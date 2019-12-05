---
title: 從 Azure CLI 建立用來裝載應用程式的 Azure App Service
description: 教學課程第 3 部分：建立 App Service
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: afac5aa2f610384b537c1f235b99cd29e6ff86d0
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466777"
---
# <a name="create-the-app-service"></a>建立 App Service

[上一步：建立應用程式](tutorial-vscode-azure-cli-node-02.md)

在此步驟中，您會使用 Azure CLI 來建立 Azure App Service 以裝載您的應用程式碼。

1. 在終端或命令提示字元中，使用下列命令來建立 App Service 的**資源群組**。 資源群組基本上是 Azure 中應用程式資源的命名集合，例如網站、資料庫、Azure Functions 等等。

    ```bash
    az group create --name myResourceGroup --location westus
    ```

    上述命令會在 `westus` 資料中心內建立名為 `myResourceGroup` 的資源群組。 如有必要，您可以變更這些值。

    一旦命令成功執行，它就會顯示 JSON 輸出，內含資源群組的詳細資料。

1. 執行下列命令，以設定後續命令的預設資源群組和區域。 這麼做可避免每次都需要指定這些值。 (此命令沒有成功的輸出。)

    ```bash
    az configure --defaults group=myResourceGroup location=westus
    ```

1. 執行下列命令來建立 **App Service 方案**，以定義 App Service 所使用的基礎虛擬機器：

    ```bash
    az appservice plan create --name myPlan --sku F1
    ```

    上述命令會指定免費主控方案 (`--sku F1`)，其使用共用虛擬機器並將方案命名為 `myPlan`。 同樣地，此命令會在成功時顯示 JSON 輸出。

1. 執行下列命令來建立 App Service，以變成 URL (`http://<your_app_name>.azurewebsites.net`) 的唯一名稱取代 `<your_app_name>`。 請注意，PowerShell 命令稍有不同。 `--runtime "node|6.9"` 引數會告訴 Azure 在伺服器上使用節點版本 6.9.x。

    ```bash
    az webapp create --name <your_app_name> --plan myPlan --runtime "node|6.9"
    ```

    > [!TIP]
    > 您也可以在 `package.json` 中陳述所需的節點版本。 Azure 會在部署期間套用此設定。 例如，下列 `package.json` 項目會告訴 Azure 至少使用 Node 7.0.0：
    >
    > ``` json
    > "engines": {
    >     "node": ">7.0.0"
    > },
    > ```

1. 執行下列命令，將瀏覽器開啟至新建立的 App Service，再次以您使用的名稱取代 `<your_app_name>`：

    ```bash
    az webapp browse --name <your_app_name>
    ```

1. 由於您還沒有為應用程式部署任何自訂程式碼，因此瀏覽器應會顯示預設頁面：

    ![預設 App Service 頁面](media/azure-cli/azure-default-page.png)

> [!div class="nextstepaction"]
> [我已建立 App Service](tutorial-vscode-azure-cli-node-04.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=create-website)
