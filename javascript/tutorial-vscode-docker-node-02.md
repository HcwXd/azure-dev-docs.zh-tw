---
title: 從 Visual Studio Code 使用容器登錄
description: 教學課程第 2 部分：使用容器登錄
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 7d51e8011824ec19e9530f9bc94bcb2ce07f2851
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466637"
---
# <a name="use-a-container-registry"></a>使用容器登錄

[上一步：簡介和必要條件](tutorial-vscode-docker-node-01.md)

在此步驟中，您會為您的應用程式映像設定合適的容器登錄。 具有容器功能的裝載服務 (例如 Azure App Service) 會從登錄中提取映像。

本教學課程會使用 [Azure Container Registry](https://azure.microsoft.com/services/container-registry/) (ACR)，這是您的映像適用的私人、安全、裝載的登錄。 不過，這裡所示的工具和程序也適用於其他登錄，像是 [Docker Hub](https://hub.docker.com/)。

## <a name="create-an-azure-container-registry"></a>建立 Azure Container Registry

1. 登入 [Azure 入口網站](https://portal.azure.com)，然後選取 [建立資源]   > [容器]   > [容器登錄]  。

    ![在 Azure 入口網站中建立容器登錄](media/deploy-containers/portal-01.png)

1. 在出現的 [建立容器登錄]  表單中，輸入適當的值：

    - [登錄名稱]  在 Azure 內必須是唯一的，且包含 5-50 個英數字元。
    - 在 [訂用帳戶]  中選取您的訂用帳戶。
    - 只有在您的訂用帳戶內，**資源群組**才需要是唯一的。
    - 在 [位置]  中，選取您附近的區域。
    - 將 [管理使用者]  設定為 [啟用]  。
    - 針對 [SKU]  選取 [基本]  。

    ![容器登錄表單的值](media/deploy-containers/portal-02.png)

1. 選取 [建立]  以建立登錄。

1. 建立登錄後，請在入口網站上開啟通知，然後針對登錄選取 [前往資源]  ：

    ![開啟新建立的登錄資源](media/deploy-containers/portal-03.png)

1. 在登錄頁面上，選取 [存取金鑰]  並記下管理員認證：

    ![Azure 入口網站上登錄此登錄的認證](media/deploy-containers/portal-04.png)

1. 在命令提示字元或終端，使用下列命令來登入 Docker，以您的登錄名稱取代 `<registry_name>`，然後以 Azure 入口網站中顯示的值取代管理使用者的 `<username>` 和 `<password>`：

    ```bash
    docker login <registry_name>.azurecr.io -u <username> -p <password>
    ```

1. 在 Visual Studio Code 中，開啟 **Docker** 總管，然後確定您剛安裝的登錄端點顯示在 [登錄]  之下：

    ![確認登錄是否出現在 Docker 總管中](media/deploy-containers/registries.png)

> [!div class="nextstepaction"]
> [我已建立登錄](tutorial-vscode-docker-node-03.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=create-registry)
