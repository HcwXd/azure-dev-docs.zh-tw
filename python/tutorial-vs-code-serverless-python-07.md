---
title: 教學課程：使用 Visual Studio Code 在 Python 中新增 Azure Functions 的儲存體繫結
description: 教學課程步驟 7：在 Python 中新增繫結，以將訊息寫入至 Azure 儲存體。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: e9f23e138dc9ddc0022199296320ff5c04e6c3d6
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172116"
---
# <a name="tutorial-add-a-storage-binding-for-azure-functions-in-python"></a>教學課程：在 Python 中新增 Azure Functions 的儲存體繫結

[上一個步驟：部署第二個函式](tutorial-vs-code-serverless-python-06.md)

_繫結_可讓您將函式程式碼連接至資源 (例如 Azure 儲存體)，而不需要撰寫任何資料存取程式碼。 繫結定義在函式 *function json* 檔案中，而且可以同時代表輸入和輸出。 一個函式可以使用多個輸入和輸出繫結，但只有一個觸發程序。 若要深入了解，請參閱 [Azure Functions 觸發程序和繫結概念](/azure/azure-functions/functions-triggers-bindings)。

在本節中，您會將儲存體繫結新增至稍早在本教學課程中建立的 HttpExample 函式。 函式會使用此繫結，在每次要求時將訊息寫入至儲存體。 有問題的儲存體會使用函式應用程式所使用的相同預設儲存體帳戶。 不過，如果打算重度使用儲存體，您會想要考慮建立個別帳戶。

1. 將 Azure Functions 專案的遠端設定同步至您的 *local.settings.json*，方法開啟 [命令選擇區] 並選取 [Azure Functions：  下載遠端設定]。 開啟 *local.settings.json*，並檢查它是否包含 `AzureWebJobsStorage` 的值。 該值是儲存體帳戶的連接字串。

1. 在 `HttpExample` 資料夾中，以滑鼠右鍵按一下 *function.json*，然後選取 [新增繫結]  ：

    ![在 Visual Studio Code 總管中新增繫結命令](media/tutorial-vs-code-serverless-python/add-binding-command.png)

1. 在 Visual Studio Code 的後續提示中，選取或提供下列值：

    | Prompt | 要提供的值 |
    | --- | --- |
    | 設定繫結方向 | out |
    | 選取向外的繫結 | Azure 佇列儲存體 |
    | 用來在程式碼中識別此繫結的名稱 | msg |
    | 要接收訊息的佇列 | outqueue |
    | 從 *local.settings.json* 選取設定 (要求儲存體連線) | AzureWebJobsStorage |

1. 在進行這些選擇之後，請驗證是否已將下列繫結新增至您的 *function.json* 檔案：

    ```json
        {
          "type": "queue",
          "direction": "out",
          "name": "msg",
          "queueName": "outqueue",
          "connection": "AzureWebJobsStorage"
        }
    ```

1. 既然您已設定繫結，就可以在函式程式碼中使用它。 新定義的繫結會再次出現在您的程式碼中，做為 *\_\_init\_\_.py* 中 `main` 函式的引數。 例如，您可以修改 HttpExample 中的 *\_\_init\_\_.py* 檔案，以符合下列情況，這會展示如何使用 `msg` 引數，利用要求中使用的名稱來撰寫有時間戳記的訊息。 註解會說明特定的變更：

    ```python
    import logging
    import datetime  # MODIFICATION: added import
    import azure.functions as func

    # MODIFICATION: the added binding appears as an argument; func.Out[func.QueueMessage]
    # is the appropriate type for an output binding with "type": "queue" (in function.json).
    def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> func.HttpResponse:
        logging.info('Python HTTP trigger function processed a request.')

        name = req.params.get('name')
        if not name:
            try:
                req_body = req.get_json()
            except ValueError:
                pass
            else:
                name = req_body.get('name')

        if name:
            # MODIFICATION: write the a message to the message queue, using msg.set
            msg.set(f"Request made for {name} at {datetime.datetime.now()}")

            return func.HttpResponse(f"Hello {name}!")
        else:
            return func.HttpResponse(
                 "Please pass a name on the query string or in the request body",
                 status_code=400
            )
    ```

1. 若要在本機測試這些變更，請按 F5 或選取 [偵錯]   > [啟動偵錯]  功能表命令，以在 Visual Studio Code 中再次啟動偵錯工具。 一如往常，[輸出]  視窗應該會在您的專案中顯示端點。

1. 在瀏覽器中，造訪 URL `http://localhost:7071/api/HttpExample?name=VS%20Code` 來建立對 HttpExample 端點的要求，也應該將訊息寫入至佇列。

1. 若要驗證是否已將訊息寫入至 "outqueue" 佇列 (同於繫結中命名的)，您可以使用下列三種方法之一：

    1. 登入 [Azure portal](https://portal.azure.com)，瀏覽至包含您函式專案的資源群組。 在該資源群組內，請尋找並瀏覽至專案的儲存體帳戶，然後瀏覽至 [佇列]  。 在該頁面上，瀏覽至 "outqueue"，這應該會顯示所有已記錄的訊息。

    1. 如[使用 Visual Studio Code 將 Functions 連接至 Azure 儲存體](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code)所述 (特別是[檢查輸出佇列](/azure/azure-functions/functions-add-output-binding-storage-queue-vs-code#examine-the-output-queue)一節)，使用與 Visual Studio 整合的 Microsoft Azure 儲存體總管瀏覽和檢查佇列。

    1. 如[查詢儲存體佇列](/azure/azure-functions/functions-add-output-binding-storage-queue-python#query-the-storage-queue)所述，使用 Azure CLI 來查詢儲存體佇列。

1. 若要在雲端中進行測試，請重新部署程式碼，方法為使用 [Azure：函式]  總管中的 [部署至函式應用程式]  。 若出現提示，請選取先前建立的函式應用程式。 一旦部署完成 (需要幾分鐘的時間！)，[輸出]  視窗就會再次顯示您可以重複測試的公用端點。

> [!div class="nextstepaction"]
> [我已新增儲存體繫結](tutorial-vs-code-serverless-python-08.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=python-functions-extension&step=07-storage-binding)
