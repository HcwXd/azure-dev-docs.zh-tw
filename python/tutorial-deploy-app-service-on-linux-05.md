---
title: 教學課程：使用 VS Code 將 Python Web 應用程式部署至 Linux 上的 Azure App Service
description: 教學課程步驟 5：部署 Web 應用程式碼
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 02d906611ac5f2a56ba85202d89c7c39058b3bc1
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466096"
---
# <a name="tutorial-deploy-your-python-web-app-to-azure-app-service-on-linux"></a>教學課程：將 Python Web 應用程式部署至 Linux 上的 Azure App Service

[上一個步驟：設定自訂啟動檔案](tutorial-deploy-app-service-on-linux-04.md)

使用下列程序，將您的 Python 應用程式部署至 Azure App Service。

1. 在 Visual Studio Code 中，開啟 [Azure：  App Service] 總管，然後選取藍色向上箭頭：

   ![在 App Service 總管中將 Web 應用程式部署至 App Service](media/deploy-azure/deploy-web-app-to-app-service-in-app-service-explorer.png)

    或者，您也可以用滑鼠右鍵按一下 App Service 名稱，然後選取 [部署至 Web 應用程式]  命令。

1. 在接下來的提示中，請提供下列詳細資料：

    - 針對 [選取要部署的資料夾]，選取您目前的應用程式資料夾。
    - 針對 [選取 Web 應用程式]，選擇您已在上一個步驟中建立的 App Service。

1. 當部署程序正在進行時，您可以在 VS Code [輸出]  視窗中檢視進度。

    ![VS Code 輸出視窗中的部署進度](media/deploy-azure/view-deployment-progress-in-visual-studio-code-output.png)

1. 當部署在幾分鐘後完成 (視需要安裝多少相依項目而定) 時，下列訊息會隨即出現。 選取 [瀏覽網站]  按鈕，以檢視執行中網站。

    ![使用 [瀏覽網站] 按鈕完成部署](media/deploy-azure/web-app-deployment-complete-with-browse-website-button.png)

    ![應用程式已成功地在 App Service 上執行](media/deploy-azure/web-app-running-successfully-on-app-service.png)

1. 若要驗證是否已部署您的檔案，請展開 [Azure：  App Service] 總管中的 App Service，然後展開 [檔案]  ：

    ![透過 App Service 總管檢查部署檔案](media/deploy-azure/expand-files-node-to-check-deployment-of-web-app-files.png)

    *antenv* 資料夾是 App Service 使用您相依項目建立虛擬環境的位置。 如果展開此節點，您可以驗證在 *requirements.txt* 命名的套件是否已安裝在 *antenv/lib/python3.7/site-packages* 中。

> [!div class="nextstepaction"]
> [我已部署應用程式](tutorial-deploy-app-service-on-linux-06.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=05-deploy-app)
