---
title: 使用 Azure Pipelines 為 MicroProfile 應用程式設定 CI/CD
description: 了解如何透過 Azure Pipelines 設定 CI/CD 發行週期，並將 MicroProfile 應用程式部署至適用於容器的 Azure Web App。
services: devops
documentationcenter: MicroProfile
author: ruyakubu
manager: brunoborges
ms.author: ruyakubu
ms.date: 07/31/2019
ms.tgt_pltfrm: multiple
ms.topic: tutorial
ms.workload: web
ms.openlocfilehash: cdd704626b51105f93c19378511f4a267cb56649
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812236"
---
# <a name="cicd-for-microprofile-apps-using-azure-pipelines"></a>使用 Azure Pipelines 為 MicroProfile 應用程式設定 CI/CD

本教學課程說明如何輕鬆地設定 Azure Pipelines 持續整合和持續部署 (CI/CD) 發行週期，以將 [MicroProfile](http://microprofile.io) Java EE 應用程式部署至適用於容器的 Azure Web App。 本教學課程中的 MicroProfile 應用程式會使用 [Payara Micro](https://www.payara.fish/payara_micro) 基底映像來建立 WAR 檔案。 

```Dockerfile
FROM payara/micro:5.182
COPY target/*.war $DEPLOY_DIR/ROOT.war
EXPOSE 8080
```
您可建置 Docker 映像，並將容器映像推送至 Azure Container Registry (ACR)，開始進行 Azure Pipelines 容器化程序。 您可建立 Azure Pipelines 發行管線並將容器映射部署至 Web 應用程式，以完成此程序。

## <a name="prerequisites"></a>必要條件

1. 在 [Azure 入口網站](https://portal.azure.com)中，建立 [Azure Container Registry](https://azure.microsoft.com/services/container-registry)。
   
1. 在 Azure 入口網站中，建立[容器的 Azure Web App](https://azure.microsoft.com/services/app-service/containers/)。 針對 [OS]  選取 [Linux]  ，並針對 [設定容器]  選取 [快速入門]  作為 [影像來源]  。  
   
1. 從範例 GitHub 存放庫 (網址為 [https://github.com/Azure-Samples/microprofile-hello-azure](https://github.com/Azure-Samples/microprofile-hello-azure)) 複製並儲存複製 URL。
   
1. 註冊或登入您的 [Azure DevOps](https://dev.azure.com) 組織，並建立新的[專案](/vsts/organizations/projects/create-project)。 
   
1. 將範例 GitHub 存放庫匯入 Azure Repos：
   
   1. 從您的 Azure DevOps 專案頁面，選取左側導覽中的 [存放庫]  。
   1. 在 [匯入存放庫]  之下，選取 [匯入]  。 
   1. 在 [複製 URL]  之下，輸入您儲存的 Git 複製 URL，然後選取 [匯入]  。
  
## <a name="create-a-build-pipeline"></a>建立組建管線

每次 Java EE 來源應用程式中有認可時，Azure Pipelines 中的持續整合組建管線就會自動執行所有組建工作。 在此範例中，Azure Pipelines 會使用 Maven 來建置 Java MicroProfile 專案。

1. 從您的 Azure DevOps 專案頁面，選取左側導覽中的 [管線]   > [組建]  。 
   
1. 選取 [新增管線]  。
   
1. 選取 [使用傳統編輯器來建立管線 (不使用 YAML)]  。 
   
1. 確定您的專案名稱和匯入的 GitHub 存放庫會現在欄位中，然後選取 [繼續]  。
   
1. 從範本清單中選取 [Maven]  ，然後選取 [套用]  。
   
1. 在右窗格中，確定 [託管 Ubuntu 1604]  出現在 [代理程式集區]  下拉式清單中。
   
   > [!NOTE]
   > 此設定可讓 Azure Pipelines 知道要使用的組建伺服器。  您也可以使用私人的自訂組建伺服器。
   
1. 若要設定持續整合的管線，請選取左窗格上的 [觸發程式]  索引標籤，然後選取 [啟用持續整合]  旁的核取方塊。  
   
1. 在頁面頂端，選取 [儲存並排入佇列]  旁的下拉式清單，然後選取 [儲存]  。 

   ![啟用持續整合](media/cicd-microprofile/continuous-integration.png)

## <a name="create-a-docker-build-image"></a>建立 Docker 組建映像

Azure Pipelines 會使用 Dockerfile 搭配來自 Payara Micro 的基礎映像，建立 Docker 映像。  

1. 選取 [工作]  索引標籤，然後選取 [代理程式作業 1 ] 旁的加號 **+** 來新增工作。 
   
   ![新增工作](media/cicd-microprofile/add-task.png)
   
1. 在右窗格中，從範本清單中選取 [Docker]  ，然後選取 [新增]  。 
   
1. 在左窗格中選取 [buildAndPush]  ，然後在右窗格的 [顯示名稱]  欄位中輸入描述。
   
1. 在 [容器存放庫]  底下，選取 [Container Registry] 欄位旁的 [新增]。   
   
1. 填寫 [新增 Docker Registry 服務連線]  對話方塊，如下所示：
   
   |欄位|值|
   |---|---|
   |**登錄類型**|選取 [Azure Container Registry]  。|
   |連線名稱 |輸入連線的名稱。|
   |**Azure 訂用帳戶**|從下拉式清單中選取您的 Azure 訂用帳戶，如有必要，請選取 [授權]  。|
   |**Azure Container Registry**|從下拉式清單中選取您的 Azure Container Registry 名稱。| 
   
1. 選取 [確定]  。
   
   ![新增 Docker Registry 服務連線](media/cicd-microprofile/dockerconnection.png)
   
   > [!NOTE]
   > 如果您使用 Docker Hub 或其他登錄，請選取 [Docker Hub]  或 [其他]  ，而不是 [登錄類型]  旁的 [Azure Container Registry]  。 然後提供您容器登錄的認證和連線資訊。
   
1. 在 [命令]  底下，從 [命令]  下拉式清單中選取 [組建]  。
   
1. 選取 [Dockerfile]  欄位旁邊的省略符號 **...** ，瀏覽至 GitHub 存放庫中的 [Dockerfile]  並加以選取，然後選取 [確定]  。 
   
   ![選取 Dockerfile](media/cicd-microprofile/selectdockerfile.png)
   
1. 在 [標記]  底下，於新的一行上輸入「最新」  標記。 
   
1. 在頁面頂端，選取 [儲存並排入佇列]  旁的下拉式清單，然後選取 [儲存]  。 

## <a name="push-the-docker-image-to-acr"></a>將 Docker 映像推送至 ACR

Azure Pipelines 將 Docker 映射推送至您的 Azure Container Registry，並使用它以容器化 Java Web 應用程式的形式執行 MicroProfile API 應用程式。

1. 由於您在 Azure Pipelines 中使用 Docker，因此請重複[建立 Docker 組建映像](#create-a-docker-build-image)底下的步驟，以建立另一個 Docker 範本。 這次在 [命令]  下拉式清單中選取 [推送]  。
   
1. 選取 [儲存並排入佇列]  旁的下拉式清單，然後選取 [儲存並排入佇列]  。 
   
1. 在 [執行管線]  快顯視窗中，確定已在 [代理程式集區]  底下選取 [託管 Ubuntu 1604]  ，然後選取 [儲存並執行]  。 
   
1. 組建完成之後，您可以選取 [組建]  頁面上的超連結來確認組建成功，並查看其他詳細資料。
   
   ![選取組建超連結](media/cicd-microprofile/checkbuild.png)

## <a name="create-a-release-pipeline"></a>建立發行管線

Azure Pipelines 持續發行管線會在組建成功時，自動觸發部署至目標環境 (例如 Azure)。 您可以針對開發、測試、預備或生產等環境建立發行管線。

1. 在您的 Azure DevOps 專案頁面上，選取左側導覽中的 [管線]   > [發行]  。 
   
1. 選取 [新增管線]  。
   
1. 在範本清單中選取 [將 Java 應用程式部署至 Azure App Service]  ，然後選取 [套用]  。 
   
   ![選取將 Java 應用程式部署到 Azure App Service 範本](media/cicd-microprofile/selectreleasetemplate.png)
   
1. 在快顯視窗中，將 [階段 1]  變更為 [開發]  、[測試]  、[預備]  或 [生產]  之類的階段名稱，然後關閉視窗。 
   
1. 在左窗格的 [成品]  底下，選取 [新增]  將成品從組建管線連結至發行管線。 
   
1. 在右窗格中，在 [來源 (組建管線)]  的下拉式清單中選取您的組建管線，然後選取 [新增]  。
   
   ![新增組建成品](media/cicd-microprofile/addbuildartifact.png)
   
1. 選取 [生產]  階段中的超連結，以 [檢視階段工作]  。
   
   ![選取階段名稱](media/cicd-microprofile/viewstagetasks.png)
   
1. 在右窗格中，填寫表單，如下所示：
   
   |欄位|值|
   |---|---|
   |**Azure 訂用帳戶**|從下拉式清單中選取您的 Azure 訂用帳戶。|
   |**應用程式類型**|從下拉式清單中選取 [適用於容器的 Web App (Linux)]  。|
   |**App Service 名稱**|從下拉式清單中選取您的 ACR 實例。|
   |**登錄或命名空間**|在欄位中輸入您的 ACR 名稱。 例如, 輸入 *mymicroprofileregistry.azure.io*。
   |**存放庫**|輸入包含 Docker 映射的存放庫。| 
   
   ![設定階段工作](media/cicd-microprofile/configurestage.png)
   
1. 在左窗格中，選取 [將 War 部署至 Azure App Service]  ，然後在右窗格的 [標記]  欄位中輸入「最新」  標記。 
   
1. 在左窗格中選取 [在代理程式上執行]  ，然後在右窗格中，從 [代理程式集區]  下拉式清單中選取 [託管 Ubuntu 1604]  。 

## <a name="set-up-environment-variables"></a>設定環境變數

新增並定義環境變數，以在部署期間連線至容器登錄。

1. 選取 [變數]  索引標籤，然後選取 [新增]  ，為您的容器登錄 URL、使用者名稱和密碼新增下列變數。 
   
   |名稱|值|
   |---|---|
   |*registry.url*|輸入容器登錄 URL。 例如：*https:\//mymicroprofileregistry.azure.io*|
   |*registry.username*|輸入登錄的使用者名稱。|
   |*registry.password*|輸入登錄的密碼。 基於安全考量，請選取上鎖圖示來隱藏密碼值。|
   
   ![新增變數](media/cicd-microprofile/addvariables.png)
   
1. 在 [工作]  索引標籤上，選取左窗格中的 [將 War 部署至 Azure App Service]  。 
   
1. 在右窗格中，展開 [應用程式和組態設定]  ，然後選取 [應用程式設定]  欄位旁的省略符號 [...]  。
   
1. 在 [應用程式設定]  快顯視窗中，選取 [新增]  以定義並指派應用程式設定變數：
   
   |名稱|值|
   |---|---|
   |*DOCKER_REGISTRY_SERVER_URL*|*$(registry.url)*|
   |*DOCKER_REGISTRY_SERVER_USERNAME*|*$(registry.username)*|
   |*DOCKER_REGISTRY_SERVER_PASSWORD*|*$(registry.password)*|
   
1. 選取 [確定]  。
   
   ![新增和設定變數](media/cicd-microprofile/appsettings.png)
   
## <a name="set-up-continuous-deployment"></a>設定連續部署 

若要啟用持續部署： 

1. 在 [管線]  索引標籤的 [成品]  底下，選取組建成品中的閃電圖示。 
   
1. 在右窗格中，將 [持續部署觸發程序]  設定為 [已啟用]  。
   
1. 選取右上方的 [儲存]  ，然後再次選取 [儲存]  。 
   
   ![啟用持續部署觸發程序](media/cicd-microprofile/setcontinuousdeployment.png)
   
## <a name="deploy-the-java-app"></a>部署 Java 應用程式

現在您已啟用 CI/CD，修改原始程式碼可建立並自動執行組建和發行。 您也可以手動建立及執行發行，如下所示：

1. 在發行管線頁面的右上方，選取 [建立發行]  。
   
1. 在 [建立新發行]  頁面上，選取 [觸發程序階段從自動變更為手動]  之下的階段名稱。 
   
1. 選取 [建立]  。 
   
1. 選取發行名稱，將滑鼠游標移至階段或選取階段，然後選取 [部署]  。 
   
## <a name="test-the-java-web-app"></a>測試 Java Web 應用程式

成功完成部署之後，請測試您的 Web 應用程式。 

1. 從 Azure 入口網站複製 Web 應用程式 URL。
   
   ![Azure 入口網站中的 App Service 應用程式](media/cicd-microprofile/portalurl.png)
   
1. 在您的網頁瀏覽器中輸入 URL，以執行您的應用程式。 網頁應該會顯示 **Hello Azure!**
   
   ![Java Web 應用程式頁面](media/cicd-microprofile/webapp.png)

