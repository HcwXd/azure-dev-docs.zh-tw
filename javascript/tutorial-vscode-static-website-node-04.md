---
title: 從 Visual Studio Code 將靜態 Node.js 應用程式檔案部署至 Azure 儲存體
description: 教學課程第 4 部分：將檔案部署至 Azure 儲存體
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 671e600471402dbd6ca71d385a238caeee889497
ms.sourcegitcommit: 66cc8d1839dbd7cc01b33030f188e15bf5f24dae
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2019
ms.locfileid: "72916674"
---
# <a name="deploy-the-website-to-azure-storage"></a>將網站部署至 Azure 儲存體

[上一步：建立儲存體帳戶](tutorial-vscode-static-website-node-03.md)

在此步驟中，您會使用 Visual Studio Code 將先前步驟中建立的靜態網站檔案部署至 Azure 儲存體，然後裝載並提供這些檔案。

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. 在 Visual Studio Code 中，移至 [Azure 儲存體]  總管、展開您的訂用帳戶、展開您在上一個步驟中建立的 Azure 儲存體帳戶節點，然後展開 [Blob 容器]  節點。 `$web` 容器是您部署應用程式碼的位置。

   ![Azure 儲存體總管中的 Azure 儲存體節點](media/static-website/storage-nodes.png)

1. 選取 [檔案]  總管，以滑鼠右鍵按一下 _dist/my-static-app_ 資料夾，然後選擇 [部署至靜態網站]  ：

    ![部署至靜態網站命令](media/static-website/deploy-build-angular.png)

1. 出現提示時，選擇您先前建立的儲存體帳戶。

1. 部署完成時，會出現一則內含 [瀏覽至網站]  按鈕的訊息。 選取該按鈕，以開啟已部署應用程式碼的主要端點。

    ![部署完成訊息](media/static-website/deployment-complete.png)

    ![在 Azure 中執行的靜態網站](media/static-website/azure-app-angular.png)

# <a name="reacttabreact"></a>[React](#tab/react)

1. 在 Visual Studio Code 中，移至 [Azure 儲存體]  總管、展開您的訂用帳戶、展開您在上一個步驟中建立的 Azure 儲存體帳戶節點，然後展開 [Blob 容器]  節點。 `$web` 容器是您部署應用程式碼的位置。

   ![Azure 儲存體總管中的 Azure 儲存體節點](media/static-website/storage-nodes.png)

1. 選取 [檔案]  總管，以滑鼠右鍵按一下您的 _build_ 資料夾，然後選擇 [部署至靜態網站]  ：

    ![部署至靜態網站命令](media/static-website/deploy-build-react.png)

1. 出現提示時，選擇您先前建立的儲存體帳戶。

1. 部署完成時，會出現一則內含 [瀏覽至網站]  按鈕的訊息。 選取該按鈕，以開啟已部署應用程式碼的主要端點。

    ![部署完成訊息](media/static-website/deployment-complete.png)

    ![在 Azure 中執行的靜態網站](media/static-website/azure-app-react.png)

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. 在 Visual Studio Code 中，移至 [Azure 儲存體]  總管、展開您的訂用帳戶、展開您在上一個步驟中建立的 Azure 儲存體帳戶節點，然後展開 [Blob 容器]  節點。 `$web` 容器是您部署應用程式碼的位置。

   ![Azure 儲存體總管中的 Azure 儲存體節點](media/static-website/storage-nodes.png)

1. 選取 [檔案]  總管，以滑鼠右鍵按一下 _dist_ 資料夾，然後選擇 [部署至靜態網站]  ：

    ![部署至靜態網站命令](media/static-website/deploy-build-vue.png)

1. 出現提示時，選擇您先前建立的儲存體帳戶。

1. 部署完成時，會出現一則內含 [瀏覽至網站]  按鈕的訊息。 選取該按鈕，以開啟已部署應用程式碼的主要端點。

    ![部署完成訊息](media/static-website/deployment-complete.png)

    ![在 Azure 中執行的靜態網站](media/static-website/azure-app-vue.png)

---

> [!div class="nextstepaction"]
> [我的網站位於 Azure](tutorial-vscode-static-website-node-05.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
