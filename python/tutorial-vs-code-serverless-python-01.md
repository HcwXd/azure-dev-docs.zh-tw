---
title: 教學課程：使用 Visual Studio Code 在 Python 中建立和部署無伺服器 Azure Functions
description: 教學課程步驟 1：簡介和必要條件。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 0aa353d741906c887723a012bb6d4abfc719a049
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118213"
---
# <a name="tutorial-create-and-deploy-serverless-azure-functions-in-python-with-visual-studio-code"></a>教學課程：使用 Visual Studio Code 在 Python 中建立和部署無伺服器 Azure Functions

在本文中，您會使用 Visual Studio Code 和 Azure Functions 擴充功能，搭配 Python 建立無伺服器 HTTP 端點，並同時將連線 (或「繫結」) 新增至儲存體。

Azure Functions 會在無伺服器環境中執行您的程式碼，而不需要佈建虛擬機器或發佈 Web 應用程式。 適用於 Visual Studio Code 的 Azure Functions 擴充功能可透過自動處理許多設定問題，來大幅簡化使用 Functions 的程序。

如果您在本教學課程中的任何步驟遇到問題，我們很樂意聆聽細節。 請使用每篇文章結尾處的 **[我遇到問題]** 按鈕來提交意見反應。

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [搭配 Azure Functions 擴充功能的 Visual Studio Code](#visual-studio-code-python-and-the-azure-functions-extension)。
- [Azure Functions Core Tools](#azure-functions-core-tools)。

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)免費的 30 天帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

### <a name="visual-studio-code-python-and-the-azure-functions-extension"></a>Visual Studio Code、Python 和 Azure Functions 擴充功能

請安裝下列軟體：

- Azure Functions 所需的 Python 3.7 或 Python 3.6。 [Python 3.7.5](https://www.python.org/downloads/release/python-375/) 和 [Python 3.6.8](https://www.python.org/downloads/release/python-368/) 是最新的相容版本。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Python 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，如 [Visual Studio Code Python 教學課程 - 必要條件](https://code.visualstudio.com/docs/python/python-tutorial)所述。
- [Azure Functions 擴充功能](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)。 如需一般資訊，請造訪 [vscode-azurefunctions GitHub 存放庫](https://github.com/Microsoft/vscode-azurefunctions)。

> [!NOTE]
> Azure Functions 擴充功能隨附於 [Azure Tools 擴充套件](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack)。

### <a name="azure-functions-core-tools"></a>Azure Functions Core Tools

遵循[使用 Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 上適用於您作業系統的指示。 這些工具本身是以 .NET Core 撰寫，而 Core Tools 套件最好是使用 Node.js 套件管理員 (npm) 來安裝，這也是為什麼您目前需要安裝 .NET Core 和 node.js 的原因，即使是 Python 程式碼也一樣。 不過，您可以使用上述文件所述的「擴充功能搭售方案」來略過 .NET Core 需求。 不論是哪種情況，您都只需要安裝這些元件一次，然後 Visual Studio Code 便會自動提示您安裝任何更新。

### <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

### <a name="verify-prerequisites"></a>驗證必要條件

若要驗證是否已安裝所有的 Azure Functions 工具，請開啟 Visual Studio Code 命令選擇區 (**F1**)、選取 **[終端機：建立新的整合式終端]** 命令，一旦終端機開啟，請執行 `func` 命令：

![檢查 Azure Functions 核心工具必要條件](media/tutorial-vs-code-serverless-python/check-azure-functions-tools-prerequisites-in-visual-studio-code.png)

以 Azure Functions 標誌開頭的輸出 (您需要向上捲動輸出) 表示 Azure Functions Core Tools 存在。

如果無法辨識 `func` 命令，請驗證您安裝 Azure Functions Core Tools 的資料夾是否已包含在 PATH 環境變數中。

> [!div class="nextstepaction"]
> [我已登入 Azure](tutorial-vs-code-serverless-python-02.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=01-verify-prerequisites)
