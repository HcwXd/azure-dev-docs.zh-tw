---
title: 從 Azure App Service 串流記錄
description: 教學課程第 5 部分：檢視記錄
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 8a173bbb7f53de2189e0ecb99b851d77704ff92d
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466761"
---
# <a name="stream-logs-from-app-service"></a>從 App Service 串流記錄

[上一步：部署應用程式](tutorial-vscode-azure-cli-node-04.md)

在此步驟中，您會從執行中的 App Service 檢視 (或「追蹤」) 記錄。 對網站程式碼中 `console.log` 的所有呼叫都會顯示於終端。

1. 執行下列命令以開始記錄，以 App Service 的名稱取代 `<your_app_name>`：

    ```bash
    az webapp log tail --name <your_app_name>
    ```

1. 幾秒鐘之後，應會顯示訊息，指出您已連線至記錄串流服務。

    ```bash
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    ```

1. 請在瀏覽器中重新整理網頁數次，以產生其他輸出：

    ```bash
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    ```

1. 按 **Ctrl**+**C** 以結束記錄工作階段。

> [!div class="nextstepaction"]
> [我看到記錄](tutorial-vscode-azure-cli-node-06.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)
