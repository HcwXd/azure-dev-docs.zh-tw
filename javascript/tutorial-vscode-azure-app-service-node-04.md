---
title: 將 Azure App Service 中的記錄串流至 Visual Studio Code
description: 教學課程第 4 部分：檢視或追蹤記錄。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 8484efc27120d374738a933244a3fce71c7c6acb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686209"
---
# <a name="stream-logs-from-azure-app-service"></a>從 Azure App Service 串流記錄

[上一步：部署網站](tutorial-vscode-azure-app-service-node-03.md)

在此步驟中，您將了解如何透過呼叫 `console.log` 來檢視或「追蹤」執行中網站所產生的任何輸出。 此輸出會出現在 Visual Studio Code 的 [輸出]  視窗中。

1. 在 **Azure App Service** 總管中，以滑鼠右鍵按一下應用程式，然後選擇 [開始串流記錄]  。

    ![檢視串流記錄](media/deploy-azure/view-logs.png)

1. 出現提示時，請選擇啟用記錄，然後重新啟動應用程式。

    ![提示啟用記錄並重新啟動](media/deploy-azure/enable-restart.png)

1. 重新啟動應用程式之後，VS Code 的 [輸出]  視窗隨即開啟，並且連線至可顯示輸出的記錄資料流。

    ```bash
    Connecting to log-streaming service...
    2019-09-20 17:33:51.428 INFO  - Container msdocs-vscode-node_2 for site msdocs-vscode-node initialized successfully.
    2019-09-20 17:33:56.500 INFO  - Container logs
    ```

1. 請在瀏覽器中重新整理網頁數次，以查看其他記錄輸出。

> [!div class="nextstepaction"]
> [我看到記錄](tutorial-vscode-azure-app-service-node-05.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
