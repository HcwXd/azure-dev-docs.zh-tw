---
title: 教學課程：使用 Visual Studio Code 將 Docker 容器部署至 Azure App Service
description: 教學課程步驟 1：簡介和必要條件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: f6cdd345fddf0123cb26549ddbc498f156737799
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172302"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>教學課程：使用 Visual Studio Code 將 Docker 容器部署至 Azure App Service

本文會逐步解說下列程序：使用 Visual Studio Code 將容器登錄中的容器映像部署至 [Azure App Service](https://azure.microsoft.com/services/app-service/containers/)，全都在 Visual Studio Code 內進行。

如果您在本教學課程中的任何步驟遇到問題，我們很樂意聆聽細節。 請使用每篇文章結尾處的 **[我遇到問題]** 連結來提交意見反應。

## <a name="prerequisites"></a>必要條件

- [Azure 帳戶](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- 已上傳至容器登錄的適當容器。 例如，有關使用 Python Web 應用程式建立容器，並將其新增至登錄的詳細資料，可在[建立容器](https://code.visualstudio.com/docs/python/tutorial-create-containers)找到。
- [適用於 VS Code 的 Azure App Service 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [適用於 VS Code 的 Docker 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。

## <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已登入 Azure](tutorial-deploy-containers-02.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)
