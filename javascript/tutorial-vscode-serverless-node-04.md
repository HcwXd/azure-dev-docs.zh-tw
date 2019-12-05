---
title: 從 Visual Studio Code 部署 Azure Functions 應用程式
description: 教學課程第 4 部分：將 Functions 應用程式部署至雲端。
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 690477392fffda4cd94d7271b061c195ccceb42b
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467106"
---
# <a name="deploy-the-functions-app"></a>部署 Functions 應用程式

[上一步：在本機測試函式](tutorial-vscode-serverless-node-03.md)

1. 在 VS Code 中，選取 Azure 標誌以開啟 [Azure 總管]  ，然後在 [Functions]  底下，選取藍色向上箭號來部署您的應用程式：

    ![部署至 Azure Functions 命令](media/functions-extension/deploy-app.png)

    或者，您也可以藉由開啟 [命令選擇區]  (**F1**)，輸入 'deploy to function app'，然後執行以下命令來部署：[Azure Functions:  部署至函式應用程式] 命令。

1. 出現 [在 Azure 中選取函式應用程式]  提示時，選擇 [在 Azure 中建立新的函式應用程式]  。

1. 出現下一個提示時，為您的函式應用程式輸入全域唯一名稱，然後按 **Enter**。 函式應用程式名稱的有效字元為 'a-z'、'0-9' 和 '-'。

1. 出現下一個提示時，選取接近您的 Azure [區域](https://azure.microsoft.com/regions/)。

1. **Azure Functions** 的 VS Code [輸出]  面板會顯示進度：

    ![顯示部署進度的 VS Code 輸出面板](media/functions-extension/deploy-progress.png)

1. 完成部署後，移至 **Azure Functions** 總管、展開您 Azure 訂用帳戶的節點、展開您 Functions 應用程式的節點，然後展開 [Functions (唯讀)]  。 以滑鼠右鍵按一下函式名稱並選取 [複製函式 Url]  ：

    ![複製函式 URL 命令](media/functions-extension/copy-function-url-command.png)

1. 將 URL 貼到瀏覽器中，然後附加 `?name=<yourname>` 引數。 然後，瀏覽器應會顯示在雲端中執行的函式：

    ![雲端中執行的函式](media/functions-extension/remote-test-browser.png)

1. 如果您想要，請在 *index.js*中對函式程式碼進行一些變更，或使用其他觸發程序來新增其他函式。 在本機測試之後，如先前步驟再次部署程式碼，以在雲端中測試這些變更。

    > [!TIP]
    > 部署時，會部署整個 Functions 應用程式，因此會一次部署對所有個別 Functions 的變更。

> [!div class="nextstepaction"]
> [我已部署函式應用程式](tutorial-vscode-serverless-node-05.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=deploy-app)
