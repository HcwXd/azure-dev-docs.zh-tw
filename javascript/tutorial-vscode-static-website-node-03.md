---
title: 從 Visual Studio Code 建立靜態 Node.js 網站的 Azure 儲存體帳戶
description: 教學課程第 3 部分：建立 Azure 儲存體帳戶
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 44e6479379fff3ddf1012cdb61cf73440cad346e
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685973"
---
# <a name="create-an-azure-storage-account"></a>建立 Azure 儲存體帳戶

[上一步：建立應用程式](tutorial-vscode-static-website-node-02.md)

在此步驟中，您會建立一個 Azure 儲存體帳戶，作為具有內建網頁伺服器的簡單檔案存放區 (或 CDN)。 這個內建伺服器讓 Azure 儲存體成為快速裝載靜態網站的絕佳選擇。

1. 從上一個步驟中建立的 `my-react-app` 資料夾，啟動 Visual Studio Code，讓它自動開啟該資料夾：

    ```bash
    code .
    ```

1. 在 VS Code 中，選取 Azure 標誌來開啟 **Azure** 總管。 在 [Azure 儲存體]  底下，以滑鼠右鍵按一下您的 Azure 訂用帳戶，然後選擇 [建立儲存體帳戶]  ：

    ![在 VS Code 中建立儲存體帳戶](media/static-website/create-storage-account.png)

1. 出現 [輸入新儲存體帳戶的名稱] 提示時，為您的儲存體帳戶輸入全域唯一名稱，然後按 Enter。 應用程式名稱的有效字元為 'a-z' 和 '0-9'。

1. 出現 [選取資源群組] 提示時，選取 [建立新的資源群組]  並接受預設名稱。

1. 出現 [選取位置] 提示時，選擇您附近的[區域](https://azure.microsoft.com/regions/)。

1. 建立儲存體帳戶時，進度會出現在 VS Code 的 [輸出]  面板中：

1. 儲存體帳戶完成後，以滑鼠右鍵按一下該帳戶，然後選取 [設定靜態網站]  。 啟用靜態網站裝載，表示 Azure 儲存體會自動為您的索引文件和任何其他靜態資產提供服務。

    ![建立儲存體帳戶](media/static-website/configure-static-website.png)

1. 出現提示時，請針對索引文件名稱和 404 錯誤文件名稱輸入 *index.html*。 我們會使用錯誤文件的 *index.html*，因為 React、Angular 和 Vue 等新式單頁應用程式 (SPA) 會處理用戶端錯誤。 若為傳統靜態網站，請使用自訂 404 錯誤頁面。

> [!div class="nextstepaction"]
> [我已建立儲存體容器](tutorial-vscode-static-website-node-04.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
