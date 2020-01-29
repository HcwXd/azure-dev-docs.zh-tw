---
title: 教學課程：清除 Azure Functions 中與 Python 程式碼搭配使用的資源
description: 教學課程步驟 8：清除 Azure 資源以避免產生持續費用。
ms.topic: conceptual
ms.date: 01/15/2020
ms.custom: seo-python-october2019
ms.openlocfilehash: 264c09a8d84c7115bb0a56d0455d576187695db0
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125251"
---
# <a name="tutorial-clean-up-azure-resources-for-azure-functions"></a>教學課程：清除適用於 Azure Functions 的 Azure 資源

[上一個步驟：新增儲存體繫結](tutorial-vs-code-serverless-python-07.md)

本文說明如何移除在本教學課程中建立的 Azure 資源。 您使用 Visual Studio Code 建立的 Azure 函式應用程式包含可能產生最低費用的資源。 (如需詳細資訊，請參閱 [Functions 定價](https://azure.microsoft.com/pricing/details/functions/)。)

清除資源的最佳方式就是刪除資源群組，其中包含本教學課程中使用的所有個別資源。 資源包括函數應用程式、儲存體帳戶，以及支援的 App Service 方案。

[!INCLUDE [delete-resource-group](includes/delete-resource-group.md)]

在 Visual Studio Code 中，您可能會注意到 [Azure：  函數] 總管中函數應用程式上的捷徑功能表具有 [刪除函數應用程式]  命令。 不過，此命令只會刪除函數應用程式，並將其他資源留在原處，因而可能會產生持續成本。

## <a name="next-steps"></a>後續步驟

恭喜完成本逐步解說，將 Python 程式碼部署至 Azure Functions！ 您現在已經準備好建立更多無伺服器函式。

如先前提到，您可以深入了解 Functions 擴充功能，方法為造訪其 GitHub 存放庫：[vscode-azurefunctions](https://github.com/Microsoft/vscode-azurefunctions)。 問題與投稿也一樣歡迎。

閱讀 [Azure Functions 概觀](/azure/azure-functions/functions-overview)，以探索您可以使用的不同觸發程序。

若要深入了解您可以從 Python 使用的 Azure 服務，包括資料儲存體以及 AI 和 Machine Learning 服務，請造訪 [Azure Python 開發人員中心](/azure/python/?view=azure-python)。

另外還有其他適用於 Visual Studio Code 的 Azure 擴充功能，您可能會覺得有用。 只需在 [擴充功能] 總管中搜尋 "Azure"：

![適用於 Visual Studio Code 的 Azure 擴充功能](media/tutorial-vs-code-serverless-python/azure-extensions-for-visual-studio-code.png)

一些熱門的擴充功能如下：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。 請參閱[部署至 App Service 教學課程](tutorial-deploy-app-service-on-linux-01.md)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) \(英文\)

> [!div class="nextstepaction"]
> [我已完成](https://docs.microsoft.com/python/azure/?view=azure-python)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=08-clean-up-resources)
