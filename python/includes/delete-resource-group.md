---
ms.openlocfilehash: 2f54e918e7126123ada68b696ea96f4d5f3dbb83
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125233"
---
您可以使用 [Azure 入口網站](https://portal.azure.com)或 Azure CLI 刪除資源群組：

- 在入口網站中，從左側瀏覽窗格選取 [資源群組]  、選取您已在本教學課程過程中建立的資源群組，然後使用 [刪除資源群組]  命令。

- 執行下列 Azure CLI 命令 (在本機或使用 [Cloud Shell](/cloud-shell/overview))，並將 `<resource_group>` 取代為本教學課程中使用的群組名稱：

    ```azurecli
    az group delete --name <resource_group>
    ```
