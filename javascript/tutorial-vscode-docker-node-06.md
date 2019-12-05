---
title: 從 Visual Studio Code 串流來自容器化 Node.js 應用程式的記錄
description: 教學課程步驟 5：將記錄串流至 Visual Studio Code
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 2ac930996bd910014565c4e329bec93015bd2a3a
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466524"
---
# <a name="stream-logs-into-visual-studio-code"></a>將記錄串流至 Visual Studio Code

[上一步：進行變更並重新部署](tutorial-vscode-docker-node-05.md)

在此步驟中，您將了解如何透過呼叫 `console.log` 來檢視或「追蹤」執行中網站所產生的任何輸出。 此輸出會出現在 Visual Studio Code 的 [輸出]  視窗中。

1. 在 **Azure App Service** 總管中，以滑鼠右鍵按一下應用程式，然後選擇 [開始串流記錄]  。

    ![檢視串流記錄](media/deploy-containers/stream-logs-command.png)

1. 出現提示時，請選擇啟用記錄，然後重新啟動應用程式。

    ![提示啟用記錄並重新啟動](media/deploy-azure/enable-restart.png)

1. 重新啟動應用程式後，Visual Studio Code 中的 [輸出]  面板就會開啟 (內含記錄資料流的連線)，並以訊息 `Starting Live Log Stream` 開始。

> [!div class="nextstepaction"]
> [我看到記錄](tutorial-vscode-docker-node-07.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=tailing-logs)
