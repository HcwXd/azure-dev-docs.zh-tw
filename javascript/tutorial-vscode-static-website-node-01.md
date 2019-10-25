---
title: 從 Visual Studio Code 將 Node.js 靜態網站部署至 Azure
description: 教學課程第 1 部分：簡介和必要條件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 0e5a7e12d234b56899e3c814cb577002125ea052
ms.sourcegitcommit: 2757d8bd0cc045b7d02f430d44de859f9de853f4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72587135"
---
# <a name="deploy-a-static-website-to-azure-from-visual-studio-code"></a>從 Visual Studio Code 將靜態網站部署至 Azure

在本教學課程中，您會使用 [Azure 儲存體](https://docs.microsoft.com/azure/storage)來建立靜態網站並將其部署至 Azure。 靜態網站是由 HTML、CSS、JavaScript 和其他靜態檔案 (例如影像或字型) 所組成。 靜態網站通常是以 Angular、React 或 Vue 撰寫的單頁應用程式 (或 [SPA](https://en.wikipedia.org/wiki/Single-page_application))。 不過，您可以設計應用程式，直接從「儲存體」  (而非使用網頁伺服器) 來裝載和提供這些檔案。 比起維護網頁伺服器，在儲存體中裝載更為簡單、成本更低。

> [!NOTE]
> 如果您有自己的伺服器程式碼，例如 Node.js/Express 應用程式，請改為遵循 [App Service 教學課程](tutorial-vscode-azure-app-service-node-01.md)。

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure 儲存體擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)。
- [Node.js 和 npm](https://nodejs.org/en/download)、Node.js 套件管理員。 (此需求僅用於產生範例專案。 如果您已經有應用程式碼，則不需要安裝 Node.js。)

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azurestorage">安裝 Azure 儲存體擴充功能</a>

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-static-website&mktingSource=vscode-tutorial-static-website)免費帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

## <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已安裝必要條件](tutorial-vscode-static-website-node-02.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=getting-started)
