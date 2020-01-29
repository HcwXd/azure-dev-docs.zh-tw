---
title: 將 Tomcat 應用程式遷移至 Azure Kubernetes Service 上的容器
description: 本指南描述當您想要遷移現有的 Tomcat 應用程式，以在 Azure Kubernetes Service 容器上執行時，應該注意的事項。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: da516609aaf976db929664bf0402a48f378034d3
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288607"
---
# <a name="migrate-tomcat-applications-to-containers-on-azure-kubernetes-service"></a>將 Tomcat 應用程式遷移至 Azure Kubernetes Service 上的容器

本指南描述當您想要遷移現有的 Tomcat 應用程式，以在 Azure Kubernetes Service (AKS) 上執行時，應該注意的事項。

## <a name="pre-migration-steps"></a>遷移前步驟

* [清查外部資源](#inventory-external-resources)
* [清查祕密](#inventory-secrets)
* [清查持續性使用量](#inventory-persistence-usage)
* [特殊案例](#special-cases)
* [現場測試](#in-place-testing)

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- AKS-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>動態或內部內容

對於您應用程式經常寫入和讀取的檔案 (例如暫存資料檔)，或只有應用程式才能看見的靜態檔案，您可以掛接 Azure 儲存體共用，做為永續性磁碟區。 如需詳細資訊，請參閱[在 Azure Kubernetes Service 中以動態方式建立和使用 Azure 檔案儲存體的永續性磁碟區](/azure/aks/azure-files-dynamic-pv)。

### <a name="identify-session-persistence-mechanism"></a>識別工作階段持續性機制

若要識別使用中的工作階段持續性管理員，請檢查應用程式和 Tomcat 設定中的 *context.xml* 檔案。 尋找 `<Manager>` 元素，然後記下 `className` 屬性的值。

Tomcat 的內建 [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) 實作，例如 [StandardManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Standard_Implementation) 或 [FileStore](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Nested_Components)，不是為了與分散式、縮放平台 (例如 Kubernetes) 搭配使用而設計的。 AKS 可能會在數個 Pod 之間進行負載平衡，並隨時以透明的方式重新啟動任何 Pod，因此不建議將可變動的狀態保存至檔案系統。

如果需要工作階段持續性，您必須使用將會寫入至外部資料存放區的替代 `PersistentManager` 實作，例如具有 Azure Cache for Redis 的樞紐工作階段管理員。 如需詳細資訊，請參閱 [使用 Redis 做為工作階段快取與 Tomcat 搭配](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat)。

### <a name="special-cases"></a>特殊案例

某些生產案例可能需要額外的變更，否則會產生額外的限制。 雖然這類案例可能不常出現，但是確保它們不適用於您的應用程式或可以正確地解決，這是很重要的。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>判斷應用程式是否依賴排程工作

Quartz 排程器工作或 cron 作業這類的排程作業不能與容器化的 Tomcat 部署搭配使用。 如果您的應用程式擴展，則某個排程作業可能會在每個排程期間執行一次以上。 這種情況可能會造成非預期的結果。

清查應用程式伺服器內外的任何排程工作。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>判斷您的應用程式是否包含作業系統特定的程式碼

如果您應用程式包含的任何程式碼可配合應用程式執行所在的作業系統，則必須重構您的應用程式，使其不依賴基礎作業系統。 例如，每當使用檔案系統路徑中的 `/` 或 `\`，都可能需要取代為 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 或 [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)。

#### <a name="determine-whether-memoryrealm-is-used"></a>判斷是否使用 MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) 需要保存的 XML 檔案。 在 Kubernetes 上，必須將此檔案新增至容器映像，或上傳至[可供容器使用的 共用儲存體](#identify-session-persistence-mechanism)。 `pathName` 參數必須相應地修改。

若要判斷目前是否使用 `MemoryRealm`，請檢查您的 *server.xml* 和 *context.xml* 檔案，並搜尋 `className` 屬性設定為 `org.apache.catalina.realm.MemoryRealm` 的 `<Realm>` 元素。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>判斷是否使用 SSL 工作階段追蹤

在容器化部署中，SSL 工作階段一般會在應用程式容器外部進行卸載，通常藉由輸入控制器進行。 如果您的應用程式需要 [SSL 工作階段追蹤](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)，請確定 SSL 流量會直接傳遞至應用程式容器。

#### <a name="determine-whether-accesslogvalve-is-used"></a>判斷是否使用 AccessLogValve

如果使用 [AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html)，`directory` 參數應該設定為 [掛接的 Azure 檔案儲存體共用](/azure/aks/azure-files-dynamic-pv)或其子目錄的其中一個。

### <a name="in-place-testing"></a>現場測試

在建立容器映像之前，請將您的應用程式遷移至您想要在 AKS 上使用的 JDK 和 Tomcat。 徹底測試您的應用程式，以確保相容性和效能。

### <a name="parametrize-the-configuration"></a>將設定參數化

在遷移前，您可能已在 *server.xml* 和 *context.xml* 檔案中識別出秘密和外部相依性，例如資料來源。 針對每個已識別的項目，將任何使用者名稱、密碼、連接字串或 URL 取代為環境變數。

例如，假設 *context.xml* 檔案包含下列元素：

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

在此情況下，您可以變更它，如下列範例所示：

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

## <a name="migration"></a>遷移

除了第一個步驟 (「佈建容器登錄和 AKS」) 之外，我們建議您針對想要遷移的每個應用程式 (WAR 檔案)，分別遵循下列步驟。

> [!NOTE]
> 某些 Tomcat 部署可能有多個應用程式在單一 Tomcat 伺服器上執行。 如果您的部署中有這種情況，強烈建議您在個別的 Pod 中執行每個應用程式。 這可讓您將每個應用程式的資源使用率最佳化，同時將複雜性和結合程度降至最低。

### <a name="provision-container-registry-and-aks"></a>佈建容器登錄和 AKS

建立容器登錄和 Azure Kubernetes 叢集，其服務主體對登錄具有讀者角色。 請務必針對您叢集的網路需求[選擇適當的網路模型](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```

### <a name="prepare-the-deployment-artifacts"></a>準備部署成品

複製[容器快速入門 GitHub 存放庫上的 Tomcat](https://github.com/Azure/tomcat-container-quickstart)。 它包含 Dockerfile 和 Tomcat 設定檔，以及數個建議的最佳化。 在下列步驟中，我們將概述在組建容器映像並部署至 AKS 之前，您可能需要對這些檔案進行的修改。

#### <a name="open-ports-for-clustering-if-needed"></a>如有需要，為叢集開啟連接埠

如果您想要在 AKS 上使用 [Tomcat 叢集](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html)，請確定必要的連接埠範圍顯示在 Dockerfile 中。 若要在 `server.xml` 中指定伺服器 IP 位址，使用的值範圍務必從容器啟動時初始化的變數到 Pod 的 IP 位址。

或者，工作階段狀態可以[保存至替代位置](#identify-session-persistence-mechanism)，以便可跨複本使用。

若要判斷您的應用程式是否使用叢集，請尋找 *server.xml* 檔案中 `<Host>` 或 `<Engine>` 元素內的 `<Cluster>` 元素。

#### <a name="add-jndi-resources"></a>新增 JNDI 資源

編輯 *server.xml*，以新增您在遷移前步驟中準備的資源，例如資料來源。

例如：

```xml
<!-- Global JNDI resources
      Documentation at /docs/jndi-resources-howto.html
-->
<GlobalNamingResources>
    <!-- Editable user database that can also be used by
         UserDatabaseRealm to authenticate users
    -->
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml"
               />

    <!-- Migrated datasources here: -->
    <Resource
        name="jdbc/dbconnection"
        type="javax.sql.DataSource"
        url="${postgresdb.connectionString}"
        driverClassName="org.postgresql.Driver"
        username="${postgresdb.username}"
        password="${postgresdb.password}"
    />
    <!-- End of migrated datasources -->
</GlobalNamingResources>
```

### <a name="build-and-push-the-image"></a>組建並推送映像

組建映像並將其上傳至 Azure Container Registry (ACR) 以供 AKS 使用的最簡單方式，就是使用 `az acr build` 命令。 此命令不需要在您的電腦上安裝 Docker。 例如，如果目前目錄中具有上述的 Dockerfile 和應用程式封裝 *petclinic.war*，則您可以使用一個步驟在 ACR 中組建容器映像：

```bash
az acr build -t "${acrName}.azurecr.io/petclinic:{{.Run.ID}}" -r $acrName --build-arg APP_FILE=petclinic.war --build-arg=prod.server.xml .
```

如果您的 WAR 檔案命名為 *ROOT.war*，則您可以省略 `--build-arg APP_FILE...` 參數。 如果您的伺服器 XML 檔案命名為 *server.xml*，則您可以省略 `--build-arg SERVER_XML...` 參數。 這兩個檔案都必須與 *Dockerfile* 位於相同的目錄中。

或者，您可以使用 Docker CLI 在本機組建映像。 這種方法可以簡化映像在初始部署至 ACR 之前的測試和微調。 不過，它需要安裝 Docker CLI，且 Docker 精靈必須執行中。

```bash
# Build the image locally
sudo docker build . --build-arg APP_FILE=petclinic.war -t "${acrName}.azurecr.io/petclinic:1"

# Run the image locally
sudo docker run -d -p 8080:8080 "${acrName}.azurecr.io/petclinic:1"

# Your application can now be accessed with a browser at http://localhost:8080.

# Log into ACR
sudo az acr login -n $acrName

# Push the image to ACR
sudo docker push "${acrName}.azurecr.io/petclinic:1"
```

如需有關在 Azure 中組建及儲存容器映像的深入資訊，請參閱個別的 [Microsoft Learn 課程](/learn/modules/build-and-store-container-images/)。

### <a name="provision-a-public-ip-address"></a>佈建公用 IP 位址

如果可從內部網路或虛擬網路外部存取您的應用程式，則需要公用靜態 IP 位址。 此 IP 位址應該佈建在叢集的節點資源群組內。

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```

### <a name="deploy-to-aks"></a>部署到 AKS

[建立並套用您的 Kubernetes YAML 檔案](/azure/aks/kubernetes-walkthrough#run-the-application)。 如果您要建立外部負載平衡器 (無論是針對您的應用程式還是針對輸入控制器)，請務必提供上一節中所佈建的 IP 位址，做為 `LoadBalancerIP`。

包括[外部化參數做為環境變數](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)。 請不要包括秘密 (例如密碼、API 金鑰和 JDBC 連接字串)。 [Configure KeyVault FlexVolume](#configure-keyvault-flexvolume) 一節中涵蓋了秘密。

### <a name="configure-persistent-storage"></a>設定永續性儲存體

如果您的應用程式需要非動態儲存體，請設定一或多個[永續性磁碟區](/azure/aks/azure-disks-dynamic-pv)。

您可能想要[使用 Azure 檔案儲存體建立](/azure/aks/azure-files-dynamic-pv)掛接至 Tomcat 記錄目錄 ( */tomcat_logs*) 的永續性儲存體，以集中保留記錄。

### <a name="configure-keyvault-flexvolume"></a>設定 KeyVault FlexVolume

[建立 Azure KeyVault](/azure/key-vault/quick-create-cli) 並填入所有必要的秘密。 然後，設定 [KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md)，讓這些秘密可供 Pod 存取。

您必須修改啟動指令碼 ([Tomcat on Containers](https://github.com/Azure/tomcat-container-quickstart) GitHub 存放庫中的 *startup.sh*)，將憑證匯入到容器上的本機金鑰儲存區。

### <a name="migrate-scheduled-jobs"></a>遷移排程作業

若要在您的 AKS 叢集上執行排程作業，請視需要定義 [Cron 作業](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)。

## <a name="post-migration-steps"></a>移轉後步驟

既然已將您的應用程式遷移至 AKS，那麼您就應該驗證它是否如預期般運作。 一旦完成，我們就會為您提供一些建議，讓您的應用程式具有更多的雲端本質。

1. 考慮[將 DNS 名稱新增](/azure/aks/ingress-static-ip#configure-a-dns-name)至配置給輸入控制器或應用程式負載平衡器的 IP 位址。

1. 考慮[為您的應用程式新增 HELM 圖表](https://helm.sh/docs/topics/charts/)。 Helm 圖可讓您將應用程式部署參數化，以供一組更多樣化的客戶使用和自訂。

1. 設計和實作 DevOps 策略。 若要維持可靠性，同時提高開發速度，請考慮[使用 Azure Pipelines 將部署和測試自動化](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)。

1. 啟用 [Azure 監視叢集](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)，以允許收集容器記錄、追蹤使用率等等。

1. 考慮透過 Prometheus 公開應用程式特定計量。 Prometheus 是在 Kubernetes 社群廣泛採用的開放原始碼計量架構。 您可以[在 Azure 監視器中設定 Prometheus 計量抓取](/azure/azure-monitor/insights/container-insights-prometheus-integration)，而不是裝載您自己的 Prometheus 伺服器，以啟用應用程式的計量匯總，以及自動回應或呈報異常狀況。

1. 設計和實作商務持續性和災害復原策略。 針對關鍵任務應用程式，請考慮[多區域部署架構](/azure/aks/operator-best-practices-multi-region)。

1. 檢閱 [Kubernetes 版本支援原則](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)。 保持[更新 AKS 叢集](/azure/aks/upgrade-cluster)以確保它一律執行支援的版本，這是您的責任。

1. 讓所有負責叢集管理和應用程式開發的小組成員，都能檢閱相關的 [AKS 最佳做法](/azure/aks/best-practices)。

1. 評估 *logging.properties* 檔案中的項目。 考慮刪除或減少部分記錄輸出以改善效能。

1. 考慮 [監視程式碼快取大小](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm) 並將 `-XX:InitialCodeCacheSize` 和 `-XX:ReservedCodeCacheSize` 參數新增至 Dockerfile 中的 `JAVA_OPTS` 變數，以進一步最佳化效能。
