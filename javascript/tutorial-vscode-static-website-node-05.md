---
title: 從 Visual Studio Code 將變更部署至靜態 Node.js 網站
description: 教學課程第 5 部分：進行變更並重新部署
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 73d27549e5ff1acb294f8e65fa610d0b46f56914
ms.sourcegitcommit: 2757d8bd0cc045b7d02f430d44de859f9de853f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72587294"
---
# <a name="make-changes-and-redeploy"></a>進行變更並重新部署

[上一步：部署至 Azure 儲存體](tutorial-vscode-static-website-node-04.md)

在此步驟中，您會對應用程式的原始程式碼進行簡單的變更並重新部署網站，以體驗端對端部署工作流程。

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. 在 Visual Studio Code 中，開啟 _src/app/app.component.html_ 檔案並變更第 305 行，以符合下列各項：

    ```html
    <span>Welcome To Azure</span>
    ```

1. 在終端或命令提示字元中，執行 `npm run build`。

1. 在 VS Code 中，以滑鼠右鍵按一下已更新的 _dist/my-static-site_ 資料夾，然後再次選擇 [部署至靜態網站]  。 選擇您的儲存體帳戶並確認您想要部署您的變更。 (Azure 擴充功能會在部署變更前自動刪除舊檔案，以避免快取問題。)

1. 部署完成後，請在瀏覽器中重新整理網站以觀察變更：

    ![重新部署後應用程式中的變更](media/static-website/updated-azure-app-angular.png)

# <a name="reacttabreact"></a>[React](#tab/react)

1. 在 Visual Studio Code 中，開啟 _src/app.js_ 檔案來變更第 11 行，以符合下列各項：

    ```html
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. 在終端或命令提示字元中，執行 `npm run build`。

1. 在 VS Code 中，以滑鼠右鍵按一下已更新的 _build_ 資料夾，然後再次選擇 [部署至靜態網站]  。 選擇您的儲存體帳戶並確認您想要部署您的變更。 (Azure 擴充功能會在部署變更前自動刪除舊檔案，以避免快取問題。)

1. 部署完成後，請在瀏覽器中重新整理網站以觀察變更：

    ![重新部署後應用程式中的變更](media/static-website/updated-azure-app-react.png)

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. 在 Visual Studio Code 中，開啟 _src/App.vue_ 檔案並變更第 11 行，以符合下列各項：

    ```html
    <HelloWorld msg="Welcome to Azure!" />
    ```

1. 在終端或命令提示字元中，執行 `npm run build`。

1. 在 VS Code 中，以滑鼠右鍵按一下已更新的 _dist_ 資料夾，然後再次選擇 [部署至靜態網站]  。 選擇您的儲存體帳戶並確認您想要部署您的變更。 (Azure 擴充功能會在部署變更前自動刪除舊檔案，以避免快取問題。)

1. 部署完成後，請在瀏覽器中重新整理網站以觀察變更：

    ![重新部署後應用程式中的變更](media/static-website/updated-azure-app-vue.png)

---

> [!div class="nextstepaction"]
> [我已部署變更](tutorial-vscode-static-website-node-06.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
