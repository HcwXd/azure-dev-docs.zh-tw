---
title: 教學課程：清除 Azure 資源
description: 教學課程步驟 5：清除 Azure 資源以避免產生持續費用。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 0c6688dcfb1d3c8adbf4ba2e0eb2603ce85de43c
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466067"
---
# <a name="tutorial-clean-up-azure-resources"></a>教學課程：清除 Azure 資源

[上一個步驟：串流記錄](tutorial-deploy-containers-04.md)

本文說明如何移除您使用 Visual Studio Code 將應用程式部署至 Azure App Service 時所建立的 Azure 資源。

您在本教學課程中建立的各種 Azure 資源都會產生持續費用。 若要清除它們，最好方法為造訪 [Azure 入口網站](https://portal.azure.com)、從左側瀏覽窗格選取 [資源群組]  、選取您已在本教學課程過程中建立的資源群組，然後使用 [刪除資源群組]  命令。

## <a name="next-steps"></a>後續步驟

您可以深入了解 Docker 和 App Service 擴充功能，方法為造訪其在 GitHub 上的個別存放庫：[vscode-Docker](https://github.com/Microsoft/vscode-docker) 和 [vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)。 問題與投稿也一樣歡迎。

若要深入了解您可以從 Python 使用的 Azure 服務，包括資料儲存體以及 AI 和 Machine Learning 服務，請造訪 [Azure Python 開發人員中心](https://docs.microsoft.com/python/azure/?view=azure-python)。

另外還有其他適用於 VS Code 的 Azure 擴充功能，您可能會覺得有用。 只需在 [擴充功能] 總管中搜尋 "Azure"：

![適用於 VS Code 的 Azure 擴充功能](media/deploy-containers/azure-extensions-for-visual-studio-code.png)

一些熱門的擴充功能如下：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) \(英文\)

> [!div class="nextstepaction"]
> [我已完成](https://docs.microsoft.com/python/azure/?view=azure-python)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=07-clean-up-resources)
