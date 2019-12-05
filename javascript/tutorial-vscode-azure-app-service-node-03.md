---
title: 從 Visual Studio Code 將 Node.js 應用程式部署至 Azure App Service
description: 教學課程第 3 部分：部署網站
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 937eb54e9885e3b5b9fa7be54f551945a54572cd
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467198"
---
# <a name="deploy-the-website"></a>部署網站

[上一步：建立應用程式](tutorial-vscode-azure-app-service-node-02.md)

在此步驟中，您會使用 Visual Studio Code 和 Azure App Service 擴充功能來部署 Node.js 網站。 本教學課程會使用最基本的部署模型，也就是您的應用程式會壓縮並部署至 Linux 上的 Azure App Service。

1. 部署應用程式之前，先確定它正在 `PORT` 環境變數所提供的連接埠上進行接聽：`process.env.PORT`。

1. 在您的應用程式資料夾中 (例如上一個步驟中的 `myExpressApp`)，使用下列命令啟動該資料夾中的 VS Code：

    ```bash
    code .
    ```

1. 在 VS Code 中，選取 Azure 圖示以開啟 **Azure App Service** 總管，然後選取藍色向上箭號圖示，將應用程式部署至 Azure：

    ![部署到 Web 應用程式](media/deploy-azure/deploy.png)

    > [!TIP]
    > 或者，開啟 [命令選擇區]  (**F1**)，鍵入 'deploy to web app'，然後選取 **Azure App Service:Deploy to Web App** 命令即可。

1. 出現提示時，輸入以下資訊：

    a. 選取您應用程式目前的資料夾 (如本教學課程中所用的 `myExpressApp`)。

    a. 選取 [建立新的 Web 應用程式]  。

    a. 為您的應用程式輸入全域唯一名稱，然後按 **Enter**。 應用程式名稱的有效字元為 'a-z'、'0-9' 和 '-'。

    a. 選擇 Azure 區域中您附近或接近您所需存取之其他服務的位置 (如[區域](https://azure.microsoft.com/regions/)所說明)。

    a. 選擇您的 **Node.js 版本** (建議使用 LTS)。

1. 當擴充功能建立應用程式時，進度會顯示在 VS Code 的 [輸出]  視窗中 (您可能需要選取 [Azure App Service]  輸出)。

    ![Azure App Service 的 Visual Studio Code 輸出視窗](media/deploy-azure/output-window.png)

1. 出現要您更新設定的提示時，請選取 [是]  ，如此才能在目標伺服器上執行 `npm install`。 接著會開始部署您的應用程式。

    ![已設定的部署](media/deploy-azure/server-build.png)

1. 部署開始後，系統會提示您更新工作區，讓所有後續部署都能自動以相同的 App Service Web 應用程式為目標。 請選擇 [是]  以確保您的變更會部署到正確的應用程式。

    ![已設定的部署](media/deploy-azure/save-configuration.png)

1. 完成部署後，請選取提示中的 [流覽網站]  ，以檢視您剛部署的網站。

## <a name="troubleshooting"></a>疑難排解

「您無權檢視此目錄或頁面」錯誤指出應用程式可能無法正確啟動。 檢視記錄 (如[下一步](tutorial-vscode-azure-app-service-node-04.md)所述) 有助於診斷並修正問題。 如果您無法修正該錯誤，請按一下下方的 [我遇到問題]  按鈕來聯繫我們。 為您服務是我們的榮幸！

## <a name="updating-the-website"></a>更新網站

若要部署此應用程式的變更，您可以使用相同程序並選擇現有的應用程式，而不是建立新的應用程式。

----

> [!div class="nextstepaction"]
> [我的網站位於 Azure](tutorial-vscode-azure-app-service-node-04.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
