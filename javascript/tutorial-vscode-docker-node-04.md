---
title: 從 Visual Studio Code 部署 Node.js 應用程式的容器映像
description: 教學課程第 4 部分：將映像部署到 Azure App Service
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 60eb5be0b3d4049c7955195f3bb6bc85dd2b2498
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686020"
---
# <a name="deploy-the-image-to-azure-app-service"></a>將映像部署到 Azure App Service

[上一步：建立應用程式映像](tutorial-vscode-docker-node-03.md)

在此步驟中，您會直接從 Visual Studio Code，將已推送至登錄的映像部署至 [Azure App Service](https://azure.microsoft.com/en-us/services/app-service/)。

1. 在 **DOCKER** 總管中，展開 [登錄]  之下您映像的節點，以滑鼠右鍵按一下 `:latest` 並選取 [將映像部署至 Azure App Service]  。

    ![從檔案總管部署](media/deploy-containers/deploy-image-command.png)

1. 出現提示時，請針對 App Service 提供值：

    - 此名稱在整個 Azure 中必須是唯一的。
    - 選取現有的資源群組或建立新群組。 (**資源群組**是 Azure 中應用程式資源的具名集合。)
    - 選取現有 App Service 方案或建立新方案。 (**App Service 方案**可定義用來裝載網站的實體資源。 您可以在本教學課程中使用基本或免費方案層。)

1. 部署完成時，Visual Studio Code 會顯示一則內含網站 URL 的通知：

    ![成功的部署訊息](media/deploy-containers/deploy-successful.png)

1. 您也可以在 [Docker]  區段中 Visual Studio Code 的 [輸出]  面板中查看結果：

    ![成功的部署輸出](media/deploy-containers/deploy-output.png)

1. 若要瀏覽已部署的網站，您可以按住 **Ctrl**+**按一下** [輸出]  面板中的 URL。 新的 App Service 也會出現在 Visual Studio Code 的 **AZURE** 總管中的 [APP SERVICE]  區段之下，您可在其中以滑鼠右鍵按一下網站並選取 [瀏覽網站]  。

> [!div class="nextstepaction"]
> [我的網站位於 Azure](tutorial-vscode-docker-node-05.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
