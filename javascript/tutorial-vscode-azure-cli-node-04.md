---
title: 使用 Azure CLI 將應用程式碼部署至 Azure App Service
description: 教學課程第 4 部分：部署網站
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 4911ccdf4003b44359d40c58d1b924e6bf88c829
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467170"
---
# <a name="deploy-the-app-to-app-service"></a>將應用程式部署至 App Service

[上一步：建立 App Service](tutorial-vscode-azure-cli-node-03.md)

在此步驟中，您會使用將本機 Git 存放庫推送至 Azure 的基本程序，將 Node.js 應用程式碼部署至 Azure App Service。

1. 在終端或命令提示字元中，執行下列命令來初始化本機 Git 存放庫並進行初始認可。 (*node_modules* 資料夾會被忽略，因為它在您稍早執行 Express 產生器時所建立的 *.gitignore* 檔案中指定。)

    ```bash
    git init
    git add -A
    git commit -m "Initial Commit"
    ```

1. 執行下列命令以在 Azure 中設定部署認證，並以您的認證取代 `username` 和 `pPassword`。 此命令會在成功時顯示 JSON 輸出。

    ```bash
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. 執行下列命令來擷取我們想要推送應用程式碼的 Git 端點，並以您在上一個步驟中建立 App Service 時所用的名稱取代 `<your_app_name>`：

    ```bash
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    此命令的輸出如下所示：

    ```output
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    ```

1. 執行下列命令，使用上一個步驟 (「省略使用者名稱」  ) 中的 URL，在 Git 中設定名為 `azure` 的新遠端。 使用上一個步驟中的範例，此命令會如下所示：

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. 執行下列命令，將應用程式碼從 Git 存放庫部署至 App Service。 此命令會提示您輸入認證：

    ```bash
    git push azure master
    ```

1. 當命令執行時，它會顯示來自遠端主機的一系列訊息。 當程序完成時，請重新整理您在其中開啟應用程式 URL 的瀏覽器，以查看執行中的程式碼：

    ![在 Azure 上執行的應用程式碼](media/azure-cli/remote-app.png)

> [!TIP]
> 如果您遇到 `Object #<eventemitter> has no method 'hrtime'` 錯誤，則可能需要在網站上設定節點執行階段版本。 下列命令會告訴網站使用節點版本 `6.9.1`。 如果您的網站需要不同或更新版本的節點，請指定完整語義版本 `major.minor.patch`。
>
> ```bash
> az webapp config appsettings set --name <your_app_name> --settings
> ```

> [!div class="nextstepaction"]
> [我已部署應用程式](tutorial-vscode-azure-cli-node-05.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
