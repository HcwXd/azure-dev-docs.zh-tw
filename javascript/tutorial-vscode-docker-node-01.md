---
title: 從 Visual Studio Code 將 Docker 容器部署至 Azure App Service
description: 教學課程第 1 部分：簡介和必要條件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: f37e049ab3c6dd0a01726aa9204746658540110b
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686120"
---
# <a name="deploy-containers-to-azure-app-service"></a>將容器部署到 Azure App Service

在本教學課程中，您會使用 Visual Studio Code 來建立使用 Docker 的容器化 Node.js 應用程式、將容器映像推送至登錄，然後將映像部署至 Azure App Service。

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Docker 擴充功能](vscode:extension/ms-azuretools.vscode-docker)
- [Azure App Service 擴充功能](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js 和 npm](https://nodejs.org/en/download)、Node.js 套件管理員。
- [Docker](https://www.docker.com/community-edition)。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-docker">安裝 Docker 擴充功能</a>

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">安裝 Azure App Service 擴充功能</a>

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)免費帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

## <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>驗證 Docker 安裝

在終端或命令提示字元中執行下列命令，確認您已正確安裝 Docker：

```bash
docker --version
```

輸出應該類似下面這樣：

```output
Docker Version 17.12.0-ce, build c97c6d6
```

> [!div class="nextstepaction"]
> [我已安裝 Docker 擴充功能](tutorial-vscode-docker-node-02.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
