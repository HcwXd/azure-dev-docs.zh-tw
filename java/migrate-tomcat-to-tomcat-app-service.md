---
title: 將 Tomcat 應用程式遷移至 Azure App Service 上的 Tomcat
description: 本指南描述當您想要使用 Tomcat 遷移現有的 Tomcat 應用程式，以在 Azure App Service 上執行時，應該注意的事項。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: ce1c54f0f4b28c5c0a2e11f4afc53f1dd59899c5
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288597"
---
# <a name="migrate-tomcat-applications-to-tomcat-on-azure-app-service"></a>將 Tomcat 應用程式遷移至 Azure App Service 上的 Tomcat

本指南描述當您想要使用 Tomcat 8.5 或 9.0 遷移現有的 Tomcat 應用程式，以在 Azure App Service 上執行時，應該注意的事項。

## <a name="before-you-start"></a>開始之前

如果您無法符合任何遷移前需求，請參閱下列隨附的遷移指南：

* [將 Tomcat 應用程式遷移至 Azure Kubernetes Service 上的容器](migrate-tomcat-to-containers-on-azure-kubernetes-service.md)
* 將 Tomcat 應用程式遷移至 Azure 虛擬機器 (即將推出)

## <a name="pre-migration-steps"></a>遷移前步驟

* [切換至支援的平台](#switch-to-a-supported-platform)
* [清查外部資源](#inventory-external-resources)
* [清查祕密](#inventory-secrets)
* [清查持續性使用量](#inventory-persistence-usage)
* [特殊案例](#special-cases)

### <a name="switch-to-a-supported-platform"></a>切換至支援的平台

App Service 會在特定版本的 JAVA 上提供特定版本的 Tomcat。 若要確保相容性，請先將您的應用程式遷移至其目前環境中其中一個支援的 Tomcat 和 JAVA 版本，再繼續進行其餘步驟。 請務必完整測試所產生的設定。 在這類測試中，使用 [Red Hat Enterprise Linux 8](https://portal.azure.com/#create/RedHat.RedHatEnterpriseLinux80-ARM) 做為作業系統。

#### <a name="java"></a>Java

> [!NOTE]
> 如果目前的伺服器是在不支援的 JDK (例如 Oracle JDK 或 IBM OpenJ9) 上執行，這項驗證就特別重要。

若要取得目前的 Java 版本，請登入您的實際執行伺服器，然後執行下列命令：

```bash
java -version
```

若要取得 Azure App Service 所使用的目前版本，請下載 [Zulu 8](https://www.azul.com/downloads/zulu-community/?&version=java-8-lts&os=&os=linux&architecture=x86-64-bit&package=jdk) (如果您想要使用 Java 8 執行階段)，或下載 [Zulu 11](https://www.azul.com/downloads/zulu-community/?&version=java-11-lts&os=&os=linux&architecture=x86-64-bit&package=jdk) (如果您想要使用 Java 11 執行階段)。

#### <a name="tomcat"></a>Tomcat

若要判斷目前的 Tomcat 版本，請登入您的實際執行伺服器，然後執行下列命令：

```bash
${CATALINA_HOME}/bin/version.sh
```

若要取得 Azure App Service 所使用的目前版本，請下載 [Tomcat 8.5](https://tomcat.apache.org/download-80.cgi#8.5.50) 或 [Tomcat 9](https://tomcat.apache.org/download-90.cgi)，取決於您打算在 Azure App Service 中使用的版本。

[!INCLUDE [inventory-external-resources](includes/migration/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/migration/inventory-secrets.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

<!-- App-Service-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>動態或內部內容

對於您應用程式經常寫入和讀取的檔案 (例如暫存資料檔)，或只有應用程式才能看見的靜態檔案，您可以將 Azure 儲存體掛接到 App Service 檔案系統中。 如需詳細資訊，請參閱[從 Azure 儲存體在 Linux 上的 App Service 中提供內容](/azure/app-service/containers/how-to-serve-content-from-azure-storage)。

### <a name="identify-session-persistence-mechanism"></a>識別工作階段持續性機制

若要識別使用中的工作階段持續性管理員，請檢查應用程式和 Tomcat 設定中的 *context.xml* 檔案。 尋找 `<Manager>` 元素，然後記下 `className` 屬性的值。

Tomcat 的內建 [PersistentManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html) 實作，例如 [StandardManager](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Standard_Implementation) 或 [FileStore](https://tomcat.apache.org/tomcat-8.5-doc/config/manager.html#Nested_Components)，不是為了與分散式、縮放平台 (例如 App Service) 搭配使用而設計的。 由於 App Service 可能會在數個實例之間進行負載平衡，並隨時以透明的方式重新啟動任何執行個體，因此不建議將可變動的狀態保存至檔案系統。

如果需要工作階段持續性，您必須使用將會寫入至外部資料存放區的替代 `PersistentManager` 實作，例如具有 Azure Cache for Redis 的樞紐工作階段管理員。 如需詳細資訊，請參閱 [使用 Redis 做為工作階段快取與 Tomcat 搭配](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat)。

### <a name="special-cases"></a>特殊案例

某些生產案例可能需要額外的變更，否則會產生額外的限制。 雖然這類案例可能不常出現，但是確保它們不適用於您的應用程式或可以正確地解決，這是很重要的。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>判斷應用程式是否依賴排程工作

Quartz 排程器工作或 cron 作業這類的排程作業不能與 App Service 搭配使用。 App Service 將不會阻止您在內部部署包含排程工作的應用程式。 不過，如果您的應用程式擴展，則相同的排程作業可能會在每個排程期間執行一次以上。 這種情況可能會造成非預期的結果。

清查應用程式伺服器內外的任何排程作業。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>判斷您的應用程式是否包含作業系統特定的程式碼

如果您的應用程式包含任何在主機作業系統上具有相依性的程式碼，則您必須重構它以移除這些相依性。 例如，您可能需要將檔案系統路徑中任何使用的 `/` 或 `\` 取代為 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 或 [`Paths.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)。

#### <a name="determine-whether-tomcat-clustering-is-used"></a>判斷是否使用 Tomcat 叢集

Azure App Service 上不支援 [Tomcat 叢集](https://tomcat.apache.org/tomcat-8.5-doc/cluster-howto.html)。 反之，您可以透過 Azure App Service 來設定和管理調整大小和負載平衡，而不需要 Tomcat 專屬功能。 您可以將工作階段狀態保存至替代位置，讓它可跨複本使用。 如需詳細資訊，請參閱[識別工作階段持續性機制](#identify-session-persistence-mechanism)。

若要判斷您的應用程式是否使用叢集，請尋找 *server.xml* 檔案中 `<Host>` 或 `<Engine>` 元素內的 `<Cluster>` 元素。

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>識別在實際執行伺服器上執行的所有外部處理序/精靈

您必須在其他位置進行遷移，或排除在應用程式伺服器外部執行的任何處理序，例如監視精靈。

#### <a name="determine-whether-non-http-connectors-are-used"></a>判斷是否使用非 HTTP 連接器

App Service 僅支援單一 HTTP 連接器。 如果您的應用程式需要額外的連接器，例如 AJP 連接器，請不要使用 App Service。

若要識別您應用程式所使用的 HTTP 連接器，請在 Tomcat 設定中的 *server.xml* 檔案內尋找 `<Connector>` 元素。

#### <a name="determine-whether-memoryrealm-is-used"></a>判斷是否使用 MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/realm/MemoryRealm.html) 需要保存的 XML 檔案。 在 Azure AppService 上，您必須將此檔案上傳至 */home* 目錄或其子目錄，或掛接的儲存體。 您必須相應地修改 `pathName` 參數。

若要判斷目前是否使用 `MemoryRealm`，請檢查您的 *server.xml* 和 *context.xml* 檔案，並搜尋 `className` 屬性設定為 `org.apache.catalina.realm.MemoryRealm` 的 `<Realm>` 元素。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>判斷是否使用 SSL 工作階段追蹤

App Service 會在 Tomcat 執行階段之外執行工作階段卸載。 因此，您無法使用 [SSL 工作階段追蹤](https://tomcat.apache.org/tomcat-8.5-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)。 請改用不同的工作階段追蹤模式 (`COOKIE` 或 `URL`)。 如果您需要 SSL 工作階段追蹤，請不要使用 App Service。

#### <a name="determine-whether-accesslogvalve-is-used"></a>判斷是否使用 AccessLogValve

如果您使用 [AccessLogValve](https://tomcat.apache.org/tomcat-8.5-doc/api/org/apache/catalina/valves/AccessLogValve.html)，應該將 `directory` 參數設定為 `/home/LogFiles` 或其子目錄。

## <a name="migration"></a>遷移

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

### <a name="provision-an-app-service-plan"></a>佈建 App Service 方案

從 [App Service 定價](https://azure.microsoft.com/pricing/details/app-service/linux/)的可用服務方案清單中，選取其規格符合或超出目前生產硬體規格的方案。

> [!NOTE]
> 如果您打算執行暫存/Canary 部署，或使用部署位置，App Service 方案必須包含該額外的容量。 我們建議對 Java 應用程式使用進階或更高方案。 如需詳細資訊，請參閱[在 Azure App Service 中設定預備環境](/azure/app-service/deploy-staging-slots)。

然後，建立 App Service 方案。 如需詳細資訊，請參閱[管理 Azure 中的 App Service 方案](/azure/app-service/app-service-plan-manage)。

### <a name="create-and-deploy-web-apps"></a>建立和部署 Web 應用程式

您必須針對部署至 Tomcat 伺服器的每個 WAR 檔案，在您的 App Service 方案上建立 Web 應用程式。

> [!NOTE]
> 儘管可將多個 WAR 檔案部署至單一 Web 應用程式，但這是我們非常不想要的結果。 將多個 WAR 檔案部署至單一 Web 應用程式，可防止每個應用程式根據自己的使用需求進行調整。 它也會對後續部署管線增加複雜性。 如果需要多個應用程式可在單一 URL 上使用，請考慮使用路由解決方案，例如 [Azure 應用程式閘道](/azure/application-gateway/)。

#### <a name="maven-applications"></a>Maven 應用程式

如果您的應用程式是從 Maven POM 檔案建置，請[使用 Maven 的 Webapp 外掛程式](/azure/app-service/containers/quickstart-java#configure-the-maven-plugin)，來建立 Web 應用程式並部署您的應用程式。

#### <a name="non-maven-applications"></a>非 Maven 應用程式

如果無法使用 Maven 外掛程式，您必須透過其他機制佈建 Web 應用程式，例如：

* [Azure 入口網站](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure CLI](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

一旦建立了 Web 應用程式，請使用其中一個[可用的部署機制](/azure/app-service/deploy-zip)來部署您的應用程式。

### <a name="migrate-jvm-runtime-options"></a>遷移 JVM 執行階段選項

如果您的應用程式需要特定的執行階段選項，請[使用最適當的機制來指定它們](/azure/app-service/containers/configure-language-java#set-java-runtime-options)。

### <a name="populate-secrets"></a>填入祕密

使用應用程式設定來儲存應用程式特定的任何秘密。 如果您想要在多個應用程式之間使用相同的祕密，或需要更細緻的存取原則和稽核功能，請[改用 Azure Key Vault](/azure/app-service/containers/configure-language-java#use-keyvault-references)。

### <a name="configure-custom-domain-and-ssl"></a>設定自訂網域和 SSL

如果可在自訂網域上看見您的應用程式，則必須[將您的 Web 應用程式對應至其中](/azure/app-service/app-service-web-tutorial-custom-domain)。

接著，您必須[將該網域的 SSL 憑證繫結至您的 App Service Web 應用程式](/azure/app-service/app-service-web-tutorial-custom-ssl)。

### <a name="migrate-data-sources-libraries-and-jndi-resources"></a>遷移資料來源、程式庫和 JNDI 資源

請遵循[下列步驟來遷移資料來源](/azure/app-service/containers/configure-language-java#tomcat)。

遵循[與資料來源 jar 檔案相同的步驟](/azure/app-service/containers/configure-language-java#finalize-configuration)，遷移任何其他伺服器層級的類別路徑相依性。

遷移任何其他的[共用伺服器層級 JDNI 資源](/azure/app-service/containers/configure-language-java#shared-server-level-resources)。

> [!NOTE]
> 如果您是遵循每個 Web 應用程式一個 WAR 的建議架構，請考慮將伺服器層級的類別路徑程式庫和 JNDI 資源遷移至您的應用程式。 這會大幅簡化元件治理和變更管理。

### <a name="migrate-remaining-configuration"></a>遷移其餘設定

完成上一節之後，您應該會在 */home/tomcat/conf* 中具有可自訂的伺服器設定。

複製任何其他設定 (例如[領域](https://tomcat.apache.org/tomcat-8.5-doc/config/realm.html)、[JASPIC](https://tomcat.apache.org/tomcat-8.5-doc/config/jaspic.html)) 來完成遷移

### <a name="migrate-scheduled-jobs"></a>遷移排程作業

若要在 Azure 上執行排程作業，請考慮搭配使用 [Azure Functions 與計時器觸發程序](/azure/azure-functions/functions-bindings-timer)。 您不需要將作業程式碼本身遷移至函數。 函數可以直接在您的應用程式中叫用 URL 來觸發作業。

或者，您可以建立[邏輯應用程式](/azure/logic-apps/logic-apps-overview)與[週期觸發程序](/azure/logic-apps/tutorial-build-schedule-recurring-logic-app-workflow#add-the-recurrence-trigger)來叫用 URL，而不需要在您的應用程式之外撰寫任何程式碼。

> [!NOTE]
> 若要防止惡意使用，您可能必須確定作業叫用端點需要認證。 在此情況下，觸發程序函數將需要提供認證。

### <a name="restart-and-smoke-test"></a>重新啟動和煙霧測試 (Smoke Test)

最後，您必須重新啟動 Web 應用程式，才能套用所有設定變更。 當重新啟動完成時，請驗證您的應用程式是否正確地執行。

## <a name="post-migration-steps"></a>移轉後步驟

既然已將應用程式遷移至 Azure App Service，那麼您就應該驗證它是否如預期般運作。 一旦完成，我們就會為您提供一些建議，讓您的應用程式具有更多的雲端本質。

### <a name="recommendations"></a>建議

1. 如果您選擇使用 */home* 目錄來儲存檔案，請考慮[將它取代為 Azure 儲存體](/azure/app-service/containers/how-to-serve-content-from-azure-storage)。

1. 如果您在 */home* 目錄中具有的設定包含連接字串、SSL 金鑰和其他祕密資訊，請考慮儘可能使用 [Azure Key Vault](/azure/app-service/app-service-key-vault-references) 和/或[參數插入與應用程式設定的組合](/azure/app-service/configure-common#configure-app-settings)。

1. 請考慮在沒有停機的情況下[使用部署位置](/azure/app-service/deploy-staging-slots)進行可靠的部署。

1. 設計和實作 DevOps 策略。 為了維持可靠性，同時提高開發速度，請考慮[使用 Azure Pipelines 將部署和測試自動化](/azure/devops/pipelines/ecosystems/java-webapp)。 如果使用部署位置，您可以[自動部署至位置](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot)和自動進行後續的位置交換。

1. 設計和實作商務持續性和災害復原策略。 針對關鍵任務應用程式，請考慮[多區域部署架構](/azure/architecture/reference-architectures/app-service-web-app/multi-region)。
