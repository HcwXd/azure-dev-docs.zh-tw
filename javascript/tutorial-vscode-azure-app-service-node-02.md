---
title: 從 Visual Studio Code 建立 Azure App Service
description: 教學課程第 2 部分：建立 Node.js 應用程式
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 96c786b7cc8112c36c0aff06761417a97e30bf44
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466807"
---
# <a name="create-your-nodejs-application"></a>建立您的 Node.js 應用程式

[上一步：簡介和必要條件](tutorial-vscode-azure-app-service-node-01.md)

在此步驟中，您會使用 Express 應用程式產生器來建立簡單的 Node.js 應用程式，而後可將它部署至 Azure。

您也可以從 [Visual Studio Code Node.js 教學課程](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)使用應用程式，在此情況下，您可以直接跳到 [部署應用程式](tutorial-vscode-azure-app-service-node-03.md)。

1. 在終端或命令提示字元中，使用下列命令來執行 Express 產生器，並 Scaffold 名為 "myExpressApp" 的新 Express 應用程式。 (`--view pug --git` 參數會指示產生器使用 [pug](https://pugjs.org/api/getting-started.html) 範本引擎 (先前稱為 Jade)，並建立 *.gitignore* 檔案。)

    ```bash
    npx express-generator myExpressApp --view pug -–git
    ```

1. 在 app 資料夾中執行 `npm install`，以安裝應用程式的相依性：

    ```bash
    cd myExpressApp
    npm install
    ```

1. 執行 `npm start` 來啟動伺服器：

    ```bash
    npm start
    ```

1. 開啟瀏覽器並瀏覽至 [http://localhost:3000](http://localhost:3000) 來測試應用程式。 網站應如下所示：

    ![執行 Express 應用程式](media/deploy-azure/express.png)

> [!div class="nextstepaction"]
> [我已建立 Node.js 應用程式](tutorial-vscode-azure-app-service-node-03.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
