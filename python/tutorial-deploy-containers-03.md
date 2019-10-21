---
title: 教學課程：在 Visual Studio Code 中進行變更之後，將容器重新部署至 Azure App Service
description: 教學課程步驟 3：重建和重新部署容器映像的簡單步驟。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: a8ce5de518ef4eeec2140ec82ef560f3b487783e
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278692"
---
# <a name="tutorial-redeploy-a-container-to-azure-app-service-after-making-changes"></a>教學課程：在進行變更後將容器重新部署至 Azure App Service

[上一個步驟：將映像部署至 Azure](tutorial-deploy-containers-02.md)

本文說明在 Visual Studio Code 中進行變更後，如何將容器重新部署至 Azure App Service。

因為您會無可避免地對應用程式進行變更，所以最後會重建並重新部署您的容器許多次。 幸運的是，程序很簡單：

1. 對您的應用程式進行變更，並在本機進行測試。 ([在 VS Code 中建立 Python 容器](https://code.visualstudio.com/docs/python/tutorial-create-container)教學課程說明此步驟和接下來的兩個步驟。)

1. 重建 Docker 映像。 如果您只變更應用程式碼，則組建應該只需要幾秒鐘的時間。

1. 將您的映像推送至登錄。 如果您再次只變更應用程式碼，則只需推送該小型層，而且程序通常會在幾秒內完成。

1. 在 [Azure：  App Service] 總管中，以滑鼠右鍵按一下適當的 App Service，然後選取 [重新啟動]  。 重新啟動 App Service 會自動從登錄中提取最新的容器映像。

1. 大約 15-20 秒之後，再次造訪 App Service URL 以檢查更新。

> [!div class="nextstepaction"]
> [我已進行變更並重新部署](tutorial-deploy-containers-04.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=03-make-changes-redeploy)