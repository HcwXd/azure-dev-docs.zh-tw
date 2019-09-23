---
title: 將 Azure App Service 中的容器記錄串流至 Visual Studio Code
description: 教學課程第 4 部分：檢視 Azure App Service 中的記錄來監視其行為。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: b886eee84e6e8daef772c2ba6e7290a604d18409
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019506"
---
# <a name="stream-logs-from-azure-app-service"></a>從 Azure App Service 串流記錄

[上一個步驟：進行變更並重新部署](tutorial-deploy-containers-03.md)

從 VS Code 內，您可以檢視 (或「跟蹤」) 來自 Azure App Service 上執行中網站的記錄，這會從 `print` 陳述式將任何輸出擷取至主控台，並將它們路由傳送至 VS Code 的 [輸出]  面板。

1. 在 [Azure：  App Service] 總管中，以滑鼠右鍵按一下應用程式，然後選擇 [啟動串流記錄]  。

1. 出現提示時，請回答 [是]  以啟用記錄，然後重新啟動應用程式。 一旦應用程式重新啟動，VS Code 的 [輸出] 面板便會隨即開啟，並且連線至記錄串流。

1. 幾秒鐘之後，您會看到指出您已連線至記錄串流服務的訊息。

    ```bash
    Connecting to log stream...
    2018-09-27T20:14:26  Welcome, you are now connected to log-streaming service.

    2018-09-27 20:14:59.269 INFO  - Starting container for site

    2018-09-27 20:14:59.270 INFO  - docker run -d -p 24138:8000 --name vsdocs-django-sample-container_0 -e WEBSITES_PORT=8000 -e WEBSITE_SITE_NAME=vsdocs-django-sample-container -e WEBSITE_AUTH_ENABLED=False -e WEBSITE_ROLE_INSTANCE_ID=0 -e WEBSITE_INSTANCE_ID=02c705ae24eaf5f298e553a9c2724b9fe4485707c2d1c36137cd02931091e561 -e HTTP_LOGGING_ENABLED=1 vsdocsregistry.azurecr.io/python-sample-vscode-django-tutorial:latest

    2018-09-27 20:15:06.216 INFO  - Container vsdocs-django-sample-container_0 for site vsdocs-django-sample-container initialized successfully.
    ```

1. 在應用程式內瀏覽，以查看各種 HTTP 要求的其他輸出。

> [!div class="nextstepaction"]
> [我看到記錄](tutorial-deploy-containers-05.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=04-stream-logs)
