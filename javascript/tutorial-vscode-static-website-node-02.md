---
title: 在 Visual Studio Code 中建立靜態的 Node.js 應用程式
description: 教學課程第 2 部分：建立範例應用程式
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: b57789ca26851c27a3a68c9d095f327ea64164cf
ms.sourcegitcommit: 2757d8bd0cc045b7d02f430d44de859f9de853f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72587252"
---
# <a name="create-the-app"></a>建立應用程式

[上一步：簡介和必要條件](tutorial-vscode-static-website-node-01.md)

在此步驟中，您會使用 [Angular](https://cli.angular.io/)、[React](https://github.com/facebook/create-react-app) 或 [Vue](https://cli.vuejs.org/) 的命令列介面 (CLI)來建立可部署至 Azure 的簡單應用程式。 您可以使用會產生一組靜態檔案的任何其他 JavaScript 架構，或是包含 HTML、CSS 或 JavaScript 檔案的任何資料夾。 如果您已經準備好要部署的應用程式，您可以直接跳到[建立 Azure 儲存體帳戶](tutorial-vscode-static-website-node-03.md)。

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. 藉由執行下列命令，使用 CLI 來構建名為「my-static-app」的新應用程式：

    ```bash
    npx @angular/cli new my-static-app
    ```

    當 CLI 詢問任何設定問題時，請按 Enter 鍵以選取預設選項。

1. 藉由切換至新的資料夾並執行 `npm run build` 來建置應用程式：

    ```bash
    cd my-static-app
    npm run build
    ```

1. 您的 _my-static-app_ 資料夾中現在應該有 _dist_ 資料夾。 在該 _dist_ 資料夾內，會有一個資料夾的名稱與您的專案 _my-static-app_的名稱相同。 _build/my-static-app_ 資料夾包含部署至 Azure 儲存體的 HTML、CSS 和 JavaScript 檔案。

1. 使用下列命令來執行應用程式：

    ```bash
    npm start
    ```

1. 開啟瀏覽器並瀏覽至 [http://localhost:3000](http://localhost:3000) 以確認應用程式正在執行中：

    ![執行中的範例 Angular 應用程式](media/static-website/local-app-angular.png)

1. 在終端或命令提示字元中按 **Ctrl**+**C**，以停止伺服器。

# <a name="reacttabreact"></a>[React](#tab/react)

1. 藉由執行下列命令，使用 CLI 來構建名為「my-static-app」的新應用程式：

    ```bash
    npx create-react-app my-static-app
    ```

1. 藉由切換至新的資料夾並執行 `npm run build` 來建置應用程式：

    ```bash
    cd my-static-app
    npm run build
    ```

1. 您的 _my-static-app_ 資料夾中現在應該有 _build_ 資料夾。 _build_ 資料夾包含您部署至 Azure 儲存體的 HTML、CSS 和 JavaScript 檔案。

1. 使用下列命令來執行應用程式：

    ```bash
    npm start
    ```

1. 開啟瀏覽器並瀏覽至 [http://localhost:3000](http://localhost:3000) 以確認應用程式正在執行中：

    ![執行中的範例 React 應用程式](media/static-website/local-app-react.png)

1. 在終端或命令提示字元中按 **Ctrl**+**C**，以停止伺服器。

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. 藉由執行下列命令，使用 CLI 來構建名為「my-static-app」的新應用程式：

    ```bash
    npx @vue/cli create my-static-app
    ```

當 CLI 詢問任何設定問題時，請按 Enter 鍵以選取預設選項。

1. 藉由切換至新的資料夾並執行 `npm run build` 來建置應用程式：

    ```bash
    cd my-static-app
    npm run build
    ```

1. 您的 _my-static-app_ 資料夾中現在應該有 _dist_ 資料夾。 _dist_ 資料夾包含部署至 Azure 儲存體的 HTML、CSS 和 JavaScript 檔案。

1. 使用下列命令來執行應用程式：

     ```bash
     npm run serve
     ```

1. 開啟瀏覽器並瀏覽至 [http://localhost:8080](http://localhost:8080) 以確認應用程式正在執行中：

    ![執行中的範例 Vue 應用程式](media/static-website/local-app-vue.png)

1. 在終端或命令提示字元中按 **Ctrl**+**C**，以停止伺服器。

---

> [!div class="nextstepaction"]
> [我已建立應用程式](tutorial-vscode-static-website-node-03.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
