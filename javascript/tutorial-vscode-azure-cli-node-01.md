---
title: 使用 Azure CLI 將 Node.js 應用程式部署至 Azure App Service
description: 教學課程第 1 部分：簡介和必要條件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 2c566e43bce15672d4ae73310f96c91f4a6f137a
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686181"
---
# <a name="deploy-to-azure-app-service-using-the-azure-cli"></a>使用 Azure CLI 部署至 Azure App Service

在本教學課程中，您會使用 [Azure 命令列介面 (CLI)](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) (其在所有作業系統上執行)，將 Node.js 應用程式部署至 Azure App Service。 透過 CLI，您可以建立 Azure 資源、在 Git 存放庫與 Azure 之間設定部署管線，以及檢視應用程式的 `console.log` 輸出。

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [Node.js 和 npm 6.x 或更高版本](https://nodejs.org/en/download)、Node.js 套件管理員。
- [Git](https://git-scm.com/downloads)，`git --version` 命令應在其後顯示版本號碼。
- [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli)。

您也可以使用[適用於 Visual Studio Code 的 Azure CLI 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)，它會在撰寫 Azure CLI 指令碼時提供語法顏色標示、IntelliSense (完成) 和程式碼片段。

第二個替代方式是 [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)，您可以使用 [Azure 帳戶擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account)在 Visual Studio Code 內使用該替代方式。

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-node-git&mktingSource=vscode-tutorial-node-git)免費帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

### <a name="sign-in-to-the-azure-cli"></a>登入 Azure CLI

安裝 Azure CLI 後，請從終端或命令提示字元執行下列命令：

```bash
az login
```

此命令會開啟瀏覽器視窗，在其中要求您登入 Azure。 登入後，終端視窗會顯示 JSON 輸出，其中包含您的訂用帳戶詳細資料。

## <a name="check-npm-version"></a>檢查 npm 版本

如果您已安裝 Node.js，請執行 `node -v` 命令並確認版本為 10.x 或更高版本。 如果您有較舊的版本，請[升級](https://nodejs.org/en/download/)到最新的 LTS (「長期支援」) 版本。

> [!div class="nextstepaction"]
> [我已安裝必要條件](tutorial-vscode-azure-cli-node-02.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=getting-started)
