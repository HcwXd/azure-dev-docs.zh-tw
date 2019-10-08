---
title: 從 Visual Studio Code 將 Node.js 應用程式部署至 Azure App Service
description: 教學課程第 1 部分：簡介和必要條件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 9c9c7085788f39e82eaec78b11d9c679b20c24eb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686215"
---
# <a name="deploy-to-azure-app-service-using-visual-studio-code"></a>使用 Visual Studio Code 部署至 Azure App Service

在本教學課程中，您會使用 [App Service 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)，將 Node.js 應用程式部署至 Linux 上的 Azure App Service。

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure App Service 擴充功能](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js 和 npm](https://nodejs.org/en/download)、Node.js 套件管理員。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">安裝 Azure App Service 擴充功能</a>

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)免費帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

## <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已安裝 Azure 擴充功能](tutorial-vscode-azure-app-service-node-02.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=getting-started)
