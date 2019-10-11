---
title: 在 Visual Studio Code 中建立靜態的 Node.js 應用程式
description: 教學課程第 2 部分：建立範例應用程式
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 566d166a69bbbee59726b8e381bee4a24077d8c6
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685982"
---
# <a name="create-the-app"></a>建立應用程式

[上一步：簡介和必要條件](tutorial-vscode-static-website-node-01.md)

在此步驟中，您會使用 React 公用程式 CLI ([create-react-app](https://github.com/facebook/create-react-app)) 來建立可部署至 Azure 的簡單 React 應用程式。 您也可以使用 Angular、Vue、其他架構，或包含幾個 HTML 檔案的任何資料夾。 如果您已經準備好要部署的應用程式，您可以直接跳到[建立 Azure 儲存體帳戶](tutorial-vscode-static-website-node-03.md)。

1. 執行下列命令，使用 create-react-app 工具來 Scaffold 名為 `my-react-app` 的新 React 應用程式：

    ```bash
    npm create react-app my-react-app
    ```

1. 藉由切換至新的資料夾並執行 `npm run build` 來建置應用程式：

    ```bash
    cd my-react-app
    npm run build
    ```

1. 您的 *my-react-app* 資料夾中現在應該有 *build* 資料夾。 *build* 資料夾包含您部署至 Azure 儲存體的 HTML、CSS 和 JavaScript 檔案。

1. 使用下列命令來執行應用程式：

    ```bash
    npm start
    ```

1. 開啟瀏覽器並瀏覽至 [http://localhost:3000](http://localhost:3000) 以確認應用程式正在執行中：

    ![執行中的範例 React 應用程式](media/static-website/local-app.png)

1. 在終端或命令提示字元中按 **Ctrl**+**C**，以停止伺服器。

> [!div class="nextstepaction"]
> [我已建立應用程式](tutorial-vscode-static-website-node-03.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
