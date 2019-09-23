---
title: 清除 Azure 資源
description: 教學課程步驟 8：清除 Azure 資源以避免產生持續費用。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 9c0b3b8b4a21975a849531d5c6560a291ed4b7f2
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019886"
---
# <a name="clean-up-resources"></a>清除資源

[上一個步驟：新增儲存體繫結](tutorial-vs-code-serverless-python-07.md)

您所建立的函式應用程式包含可能產生最低費用的資源 (請參閱[函式定價](https://azure.microsoft.com/pricing/details/functions/))。 若要清除資源，請以滑鼠右鍵按一下 [Azure：  函式] 總管中的函式應用程式，然後選取 [刪除函式應用程式]  。

您也可以造訪 [Azure 入口網站](https://portal.azure.com)、從左側瀏覽窗格選取 [資源群組]  、選取您已在本教學課程過程中建立的資源群組，然後使用 [刪除資源群組]  命令。

## <a name="next-steps"></a>後續步驟

恭喜完成本逐步解說，將 Python 程式碼部署至 Azure Functions！ 您現在已經準備好建立更多無伺服器函式。

如先前提到，您可以深入了解 Functions 擴充功能，方法為造訪其 GitHub 存放庫：[vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)。 問題與投稿也一樣歡迎。

閱讀 [Azure Functions 概觀](/azure/azure-functions/functions-overview.md)，以探索您可以使用的不同觸發程序。

若要深入了解您可以從 Python 使用的 Azure 服務，包括資料儲存體以及 AI 和 Machine Learning 服務，請造訪 [Azure Python 開發人員中心](/azure/python/?view=azure-python)。

另外還有其他適用於 Visual Studio Code 的 Azure 擴充功能，您可能會覺得有用。 只需在 [擴充功能] 總管中搜尋 "Azure"：

![適用於 Visual Studio Code 的 Azure 擴充功能](media/tutorial-vs-code-serverless-python/azure-extensions.png)

一些熱門的擴充功能如下：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。 請參閱[部署至 App Service 教學課程](tutorial-deploy-app-service-on-linux-01.md)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) \(英文\)

> [!div class="nextstepaction"]
> [我已完成](https://docs.microsoft.com/python/azure/?view=azure-python)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
