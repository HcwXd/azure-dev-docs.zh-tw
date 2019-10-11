---
title: 教學課程：使用 Visual Studio Code 在 Python 中部署 Azure Functions
description: 教學課程步驟 5：將 Python 函式程式碼部署至 Azure，並瞭解如何在本機專案與 Azure 之間串流記錄及同步處理設定。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 231aefd696b3f4c91e5da8156dc339f4b355c1c7
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172183"
---
# <a name="tutorial-deploy-azure-functions-in-python"></a>教學課程：在 Python 中部署 Azure Functions

[上一個步驟：在本機進行偵錯](tutorial-vs-code-serverless-python-04.md)

在這些步驟中，您會使用函式擴充功能，在 Azure 中建立函式應用程式，以及其他必要的 Azure 資源。 函式應用程式可讓您將多個函式群組為邏輯單位，以方便您管理、部署和共用資源。 對於資料和[主控方案](/azure/azure-functions/functions-scale#hosting-plan-support)，它也需要 Azure 儲存體帳戶。 所有這些資源都組織在單一資源群組內。

1. 在 [Azure：  函式] 總管中，選取 [部署至函式應用程式]  命令，或開啟 [命令選擇區] (**F1**)，然後選取 [Azure Functions：  部署至函式應用程式] 命令。 函式應用程式再次為 Python 專案在 Azure 執行的所在。

    ![[部署至函式應用程式] 命令](media/tutorial-vs-code-serverless-python/deploy-command.png)

1. 出現提示時，請選取 [在 Azure 中建立新的函式應用程式]  ，並提供在整個 Azure 中唯一的名稱 (通常會使用您的個人或公司名稱，以及其他唯一識別碼；您可以使用字母、數字和連字號)。 如果您先前已建立函式應用程式，則其名稱會出現在此選項清單中。

1. 擴充功能會執行下列動作，您可以在 Visual Studio Code 快顯訊息和 [輸出]  視窗中觀察這些動作 (此程序需要幾分鐘的時間)：

    - 使用您所提供的名稱 (移除連字號) 來建立資源群組。
    - 在該資源群組中，建立儲存體帳戶、主控方案和函式應用程式。 根據預設，會建立[取用方案](/azure/azure-functions/functions-scale#consumption-plan)。 若要在專屬方案中執行您的函式，您需要[使用進階建立選項來啟用發佈功能](/azure/azure-functions/functions-develop-vs-code)。
    - 將您的程式碼部署至函式應用程式。

    [Azure：  函式] 總管也會顯示進度：

    ![Azure 中的部署進度列指示器：函式總管](media/tutorial-vs-code-serverless-python/deploy-progress.png)

1. 一旦部署完成，Azure Functions 擴充功能便會顯示一則訊息，其中包含三個額外動作的按鈕：

    ![指出部署成功並包含其他動作的訊息](media/tutorial-vs-code-serverless-python/deployment-popup.png)

    對於 [串流記錄]  和 [上傳設定]  ，請參閱下一節。 對於 [檢視輸出]  ，請參閱隨後的步驟 5。

1. 在部署之後，[輸出]  視窗也會顯示 Azure 上的公用端點：

    ```output
    HTTP Trigger Urls:
      HttpExample: https://vscode-azure-functions.azurewebsites.net/api/HttpExample
    ```

    使用此端點來執行您在本機進行的相同測試，方法為使用 URL 參數和/或要求與要求本文中的 JSON 資料搭配。 公用端點的結果應該符合您先前在[第 4 部分](tutorial-vs-code-serverless-python-04.md)中測試的本機端點結果。

## <a name="stream-logs"></a>串流記錄

目前正在開發記錄串流的支援，如 Azure Functions 擴充功能的[問題 589](https://github.com/microsoft/vscode-azurefunctions/issues/589) 所述。 部署訊息快顯視窗中的 [串流記錄]  按鈕最終會將 Azure 上的記錄輸出連接至 Visual Studio Code。 您也可以用滑鼠右鍵按一下 Functions 專案，然後選取 [啟動串流記錄]  或 [停止串流記錄]  ，以在 **Azure Functions** 總管上啟動和停止記錄流程。

不過，這些命令目前尚無法運作。 可以改在瀏覽器中使用記錄串流，方法為執行下列命令，將 `<app_name>` 取代為 Azure 上函式應用程式的名稱：

```bash
# Replace <app_name> with the name of your Functions app on Azure
func azure functionapp logstream <app_name> --browser
```

## <a name="sync-local-settings-to-azure"></a>將本機設定同步至 Azure

部署訊息快顯視窗中的 [上傳設定]  按鈕會將您對 *local.settings.json* 檔案所做的任何變更套用至 Azure。 您也可以展開 Functions 專案節點、以滑鼠右鍵按一下[應用程式設定]  ，然後選取 [上傳本機設定]  ，在 **Azure Functions** 總管上叫用命令。 您也可以使用 [命令選擇區] 來選取 [Azure Functions：  上傳本機設定] 命令。

上傳設定會更新任何現有的設定，並新增在 *local.settings.json* 中定義的任何新設定。 上傳並不會從 Azure 中移除未在本機檔案中列出的任何設定。 若要移除這些設定，請展開 **Azure Functions** 總管中的 [應用程式設定]  節點、以滑鼠右鍵按一下設定，然後選取 [刪除設定]  。 您也可以直接在 Azure 入口網站上編輯設定。

若要將您透過入口網站或透過 **Azure Explorer** 進行的任何變更套用至 *local.settings.json* 檔案，請以滑鼠右鍵按一下[應用程式設定]  節點，然後選取 [下載遠端設定]  命令。 您也可以使用 [命令選擇區] 來選取 [Azure Functions：  下載遠端設定] 命令。

> [!div class="nextstepaction"]
> [我已部署函式](tutorial-vs-code-serverless-python-06.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=05-deploy)
