---
title: 使用 Azure CLI 建立 Node.js 應用程式以部署至 Azure
description: 教學課程第 2 部分：建立應用程式碼。
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: dba089c58cf6413263348b7bdeb975852c2e6a2f
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467187"
---
# <a name="create-the-app-code-using-express"></a>使用 Express 建立應用程式碼

[上一步：簡介和必要條件](tutorial-vscode-azure-cli-node-01.md)

在此步驟中，您會使用 [Express 產生器](https://expressjs.com/en/starter/generator.html)建立採用 [Express](https://www.expressjs.com) 的簡單 Node.js 應用程式。

1. 使用下列命令來執行 Express 產生器，並 Scaffold 名為 "myExpressApp" 的新 Express 應用程式。 (`--view pug --git` 參數會指示產生器使用 [pug](https://pugjs.org/api/getting-started.html) 範本引擎 (先前稱為 Jade)，並建立 *.gitignore* 檔案。)

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. 藉由執行下列命令，瀏覽至應用程式資料夾並安裝應用程式的相依性：

    ```bash
    cd myExpressApp
    npm install
    ```

1. 執行下列命令來啟動應用程式伺服器：

    ```bash
    npm start
    ```

1. 開啟瀏覽器並瀏覽至 [http://localhost:3000](http://localhost:3000)，以查看執行中的應用程式：

    ![在本機執行 Express 應用程式](media/azure-cli/local-app.png)

1. 當您完成應用程式測試時，請在您執行 `npm start` 的終端中按 **Ctrl**+**C** 來停止伺服器。

> [!div class="nextstepaction"]
> [我建立應用程式](tutorial-vscode-azure-cli-node-03.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=express)
