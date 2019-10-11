---
title: 教學課程：在部署至 Linux 上的 Azure App Service 之後，從 Visual Studio Code 清除資源
description: 教學課程步驟 7：清除 Azure 資源
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 4ffadce6a6895041efe6737b271d7ab11c830095
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172466"
---
# <a name="tutorial-clean-up-resources-after-deploying-to-azure-app-service-on-linux-from-visual-studio-code"></a>教學課程：在部署至 Linux 上的 Azure App Service 之後，從 Visual Studio Code 清除資源

[上一個步驟：串流記錄](tutorial-deploy-app-service-on-linux-06.md)

您所建立的 App Service 包含可能會產生費用的支援 App Service 方案。 若要清除資源，請以滑鼠右鍵按一下 [Azure：  App Service] 總管中的 App Service，然後選取 [刪除]  。

您也可以造訪 [Azure 入口網站](https://portal.azure.com)、從左側瀏覽窗格選取 [資源群組]  、選取您已在本教學課程過程中建立的資源群組，然後使用 [刪除資源群組]  命令。

## <a name="next-steps"></a>後續步驟

恭喜完成本逐步解說，將 Python 程式碼部署至 Linux 上的 App Service！

如先前提到，您可以深入了解 App Service 擴充功能，方法為造訪其 GitHub 存放庫：[vscode-azureappservice](https://github.com/Microsoft/vscode-azureappservice)。 問題與投稿也一樣歡迎。

若要深入了解您可以從 Python 使用的 Azure 服務，包括資料儲存體以及 AI 和 Machine Learning 服務，請造訪 [Azure Python 開發人員中心](https://docs.microsoft.com/python/azure/?view=azure-python)。

另外還有其他適用於 VS Code 的 Azure 擴充功能，您可能會覺得有用。 只需在 [擴充功能] 總管中搜尋 "Azure"：

![適用於 VS Code 的 Azure 擴充功能](media/deploy-containers/azure-extensions.png)

一些熱門的擴充功能如下：

- [Cosmos DB](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)
- [Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
- [Azure CLI 工具](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)
- [Azure Resource Manager (ARM) 工具](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)

> [!div class="nextstepaction"]
> [我已完成](https://docs.microsoft.com/python/azure/?view=azure-python) 

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=07-clean-up-resources)
