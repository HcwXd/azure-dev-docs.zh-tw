---
title: 使用 Azure CLI 將 Node.js 應用程式部署至 Azure 後清除資源
description: 教學課程第 7 部分：清除資源
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 6d0b9c671c4ffd0fb5d521e6247e9964d6d93980
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686108"
---
# <a name="clean-up-resources"></a>清除資源

[上一步：進行變更並重新部署](tutorial-vscode-docker-node-06.md)

您所建立的 App Service 包含可能會產生費用的支援 App Service 方案。 若要清除資源，請在終端或命令提示字元中執行下列命令：

```bash
az group delete --name myResourceGroup
```

您也可以造訪 [Azure 入口網站](https://portal.azure.com)、從左側瀏覽窗格選取 [資源群組]  、選取您已在本教學課程過程中建立的資源群組，然後使用 [刪除資源群組]  命令。

> [!div class="nextstepaction"]
> [我已完成](node-howto-deploy-web-app.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=clean-up-resources)
