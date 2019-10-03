---
title: 在 Visual Studio Code 中檢查 Azure Functions 的 Python 程式碼檔案
description: 教學課程步驟 3：了解 Azure Functions 提供的範本 Python 程式碼。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.openlocfilehash: b1ed7f69656797822b17c11bcf0f2ece0caa09e2
ms.sourcegitcommit: 86aec15e2d25b5b706092feb4271fb958c29d5fa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2019
ms.locfileid: "71707346"
---
# <a name="examine-the-code-files"></a>檢查程式碼檔案

[上一個步驟：建立函式](tutorial-vs-code-serverless-python-02.md)

在新建立的函式中，子資料夾為三個檔案： *\_\_init\_\_.py* 包含函式的程式碼、*function.json* 說明 Azure Functions 的函式，以及 *sample.dat* 是範例資料檔。 您可以刪除 *sample.dat* (如果想要的話)，因為它的存在只為了顯示，您可以將其他檔案新增至子資料夾。

讓我們首先查看 *function.json*，然後查看 *\_\_init\_\_.py* 中的程式碼。

## <a name="functionjson"></a>function.json

function.json 檔案會提供 Azure Functions 端點的必要設定資訊：

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "authLevel": "anonymous",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

`scriptFile` 屬性會識別程式碼的啟動檔案，而且該程式碼必須包含名為 `main` 的 Python 函式。 只要此處指定的檔案包含 `main` 函式，您就可以將程式碼分解成多個檔案。

`bindings` 元素包含兩個物件，一個用來描述傳入要求，另一個則用來描述 HTTP 回應。 針對傳入要求 (`"direction": "in"`)，函式會回應 HTTP GET 或 POST 要求，而且不需要驗證。 回應 (`"direction": "out"`) 是 HTTP 回應，其會傳回從 `main` Python 函式傳回的任何值。

## <a name="__initpy__"></a>\_\_init.py\_\_

當您建立新函式時，Azure Functions 會在 *\_\_init\_\_.py* 中提供預設 Python 程式碼：

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest) -> func.HttpResponse:
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
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

程式碼的重要部分如下：

- 您必須從 `azure.functions` 匯入 `func`；匯入記錄模組是選擇性的，但建議匯入。
- 必要的 `main` Python 函式會接收名為 `req` 的 `func.HttpRequest` 物件，並傳回類型 `func.HttpResponse` 的值。 您可以在 [func.HttpRequest](/python/api/azure-functions/azure.functions.httprequest?view=azure-python) 和 [func.HttpResponse](/python/api/azure-functions/azure.functions.httpresponse?view=azure-python) 參考中深入了解這些物件的功能。
- 然後，`main` 的本文會處理要求並產生回應。 在此情況下，程式碼會在 URL 中尋找 `name` 參數。 若失敗，它會檢查要求本文是否包含 JSON (使用 `func.HttpRequest.get_json`)，以及 JSON 是否包含 `name` 值 (使用 `get_json` 所傳回 JSON 物件的 `get` 方法)。
- 如果找到名稱，則程式碼會傳回附加名稱的字串 "Hello"，否則它會傳回錯誤訊息。

> [!div class="nextstepaction"]
> [我已檢查程式碼檔案](tutorial-vs-code-serverless-python-04.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=03-examine-code-files)
