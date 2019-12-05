---
title: 教學課程：使用 Visual Studio Code 將容器映像部署至 Azure App Service
description: 教學課程步驟2：將實際 Docker 映像部署至容器登錄中的 Azure App Service。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 0039b2cc9e612d7e03398e772183fe6eb81313f2
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467046"
---
# <a name="tutorial-deploy-a-container-image-to-azure-app-service"></a>教學課程：將容器映像部署至 Azure App Service

[上一個步驟：必要條件](tutorial-deploy-containers-01.md)

使用登錄中的容器映像，您可以在 VS Code 中使用 Docker 擴充功能，輕鬆地設定執行容器的 Azure App Service。

1. 在 **Docker** 總管中，依序展開 [登錄]  、登錄的節點 (例如 **Azure**)、映像名稱的節點，直到您看到具有 `:latest` 標籤的映像。

    ![在 Docker 總管中尋找映像](media/deploy-containers/find-image-to-deploy-in-docker-explorer.png)

1. 以滑鼠右鍵按一下映像，然後選取 [將映像部署至 Azure App Service]  。

    ![選取 [將映像部署至 Azure App Service] 功能表項目](media/deploy-containers/deploy-image-to-azure-app-service-with-docker-explorer.png)

1. 遵循提示來選取 Azure 訂用帳戶、選取或指定資源群組、指定區域、設定 App Service 方案 (B1 是最便宜的方案)，然後指定網站的名稱。 下列動畫說明此程序。

    ![建立映像並將其部署至 Azure App Service](media/deploy-containers/deploy-image-to-azure-app-service.gif)

    **資源群組**是一個具名集合，其中包含構成應用程式的不同資源。 藉由將應用程式的所有資源指派給單一群組，您就可以輕鬆地將這些資源當做單一單位來管理。 (如需詳細資訊，請參閱 Azure 文件中的 [Azure Resource Manager 概觀](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)。)

    **App Service 方案**定義裝載執行中容器的實體資源 (基礎虛擬機器)。 在本教學課程中，B1 是支援 Docker 容器的最便宜方案。 (如需詳細資訊，請參閱 Azure 文件中的 [App Service 方案概觀](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)。)

    App Service 的名稱在所有 Azure 中必須是唯一的，因此您通常會使用公司或個人名稱。 對於生產網站，您通常會使用個別註冊的網域名稱來設定 App Service。

1. 建立 App Service 需要幾分鐘的時間，而您會在 VS Code 的輸出面板中看到進度。

1. 完成之後，您也**必須**將名為 `WEBSITES_PORT` 的設定 (請注意複數 "WEBSITES") 新增至 App Service，以指定容器正在接聽的連接埠。 (例如，如果您使用的映像來自[在 VS Code 中建立 Python 容器](https://code.visualstudio.com/docs/python/tutorial-create-container)教學課程，則 Flask 的連接埠為 5000，而 Django 的連接埠則為8000)。 若要 `WEBSITES_PORT` 設定，請切換至 [Azure：  App Service] 總管、展開新 App Service 的節點 (如有必要，請重新整理)，然後以滑鼠右鍵按一下 [應用程式設定]  ，再選取 [新增設定]  。 出現提示時，輸入 `WEBSITES_PORT` 做為值的金鑰和連接埠號碼。

    ![將新的設定新增至 App Service 以指定連接埠](media/deploy-containers/add-new-setting-in-app-service-settings-explorer.png)

1. 當您變更設定時，App Service 會自動重新啟動。 您也可以隨時以滑鼠右鍵按一下 App Service，然後選取 [重新啟動]  。

1. 在服務重新啟動之後，瀏覽位於 `http://<name>.azurewebsites.net` 的網站。 您可以在 [輸出] 面板中的 URL 上使用 **Ctrl** + 按一下滑鼠左鍵 (或在 macOS 上，**Cmd** + 按一下滑鼠左鍵)，或以滑鼠右鍵按一下 [Azure：  App Service] 總管中的 App Service，然後選取 [瀏覽網站]  。

> [!div class="nextstepaction"]
> [我已部署映像](tutorial-deploy-containers-03.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=02-deploy-container)
