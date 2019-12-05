---
title: 在 Visual Studio Code 本機執行 Azure Functions 應用程式
description: 教學課程第 3 部分，在本機執行應用程式以進行測試。
ms.topic: conceptual
ms.date: 09/23/2019
ms.openlocfilehash: 2a7cb5e5c433c90d74cd3b7771ce90529f617fcb
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466576"
---
# <a name="test-the-function-locally"></a>在本機測試函式

[上一步：建立 Functions 應用程式](tutorial-vscode-serverless-node-02.md)

在此步驟中，您會在本機執行 Azure Functions 專案，以在部署至 Azure 前進行測試。

當您建立 Functions 應用程式時，Azure Functions 擴充功能會自動將 VS Code 啟動設定新增至您的專案，其可在 *.vscode/launch.json* 檔案中找到。 此設定會使用在 Azure 上執行的相同執行階段，因此您可在部署至雲端之前，確定您的原始程式碼能運作。

1. 在 Visual Studio Code 中，按 **F5** (或使用 [偵錯]   > [開始偵錯]  功能表命令) 啟動偵錯工具並連結至 Azure Functions 主機。 (此命令會自動使用 Azure Functions 所建立的單一偵錯設定。)

1. Functions Core Tools 的輸出會出現在 VS Code 的 [終端]  面板中。 主機啟動後，按住 **Ctrl** 並按一下輸出中顯示的本機 URL，以開啟瀏覽器並執行此函式：

    ![在本機進行偵錯時 VS Code 的 [終端] 面板中顯示的輸出](media/functions-extension/local-test-output.png)

1. 預設 HTTP 觸發程序範本所建立的程式碼會剖析 `name` 查詢參數，以自訂回應。 在您的瀏覽器中，將 `?name=<yourname>` 新增至瀏覽器中的 URL，以正確查看回應輸出：

    ![剖析 URL 參數的 HTTP 觸發程序函式](media/functions-extension/local-test-browser.png)

1. 當您的函式在本機執行時，您可在程式碼的不同部分設定中斷點。 (若要深入了解 VS Code 中的中斷點和偵錯，請參閱[偵錯](https://code.visualstudio.com/docs/editor/debugging)。)開啟 *index.js*，然後在編輯器視窗中按一下第 11 行左側的邊界。 此時會出現一個小紅點，表示中斷點。 現在從瀏覽器中的 URL 移除 `?name=` 引數。 當瀏覽器提出該要求時，VS Code 會在該中斷點上停止函式程式碼：

    ![VS Code 已在中斷點上停止](media/functions-extension/debugging-breakpoint.png)

> [!div class="nextstepaction"]
> [我已在本基執行函式應用程式](tutorial-vscode-serverless-node-04.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)
