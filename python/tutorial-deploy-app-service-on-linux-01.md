---
title: 教學課程：從 Visual Studio Code 將 Python 應用程式部署至 Linux 上的 Azure App Service
description: 教學課程步驟 1：簡介、必要條件和登入 Azure。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: d5eed3d2b1aeea92b3681ada006b3723e67f70c4
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466270"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>教學課程：從 Visual Studio Code 將 Python 應用程式部署至 Linux 上的 Azure App Service

本文會逐步引導您使用 Visual Studio Code，搭配 [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) 擴充功能將 Python 應用程式部署至 Linux 上的 Azure App Service。

如果您在本教學課程中的任何步驟遇到問題，我們很樂意聆聽細節。 請使用每篇文章結尾處的 **[我遇到問題]** 連結來提交意見反應。

> [!TIP]
> [Linux 上的 Azure App Service](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro) 會在預先定義的 Docker 容器中執行您的原始程式碼。 該容器會使用 [Gunicorn](https://gunicorn.org) Web 伺服器，搭配 Python 3.7 執行應用程式。 [設定適用於 Azure App Service 的 Linux Python 應用程式](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)會描述此容器的特性。 容器定義本身位於 [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7)。

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [搭配 Azure App Service 擴充功能的 Visual Studio Code](#visual-studio-code-python-and-the-azure-app-service-extension)。
- Python 環境

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)免費帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code、Python 和 Azure App Service 擴充功能

請安裝下列軟體：

- [Visual Studio Code](https://code.visualstudio.com/)。
- Python 和 [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) 擴充功能，如 [VS Code Python 教學課程 - 必要條件](https://code.visualstudio.com/docs/python/python-tutorial)所述。
- [Azure App Service](vscode:extension/ms-azuretools.vscode-azureappservice) 擴充功能，可從 VS Code 內提供與 Azure App Service 的互動。 如需一般資訊，請瀏覽 [App Service 擴充功能教學課程](https://code.visualstudio.com/tutorials/app-service-extension/getting-started)，並造訪 [vscode-azureappservice GitHub 存放庫](https://github.com/Microsoft/vscode-azureappservice)。

## <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已登入 Azure](tutorial-deploy-app-service-on-linux-02.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=01-verify-prerequisites)
