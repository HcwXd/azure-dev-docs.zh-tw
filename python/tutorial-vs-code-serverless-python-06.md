---
title: 教學課程：使用 Visual Studio Code 將第二個 Python 函式新增至 Azure Functions
description: 教學課程步驟6：新增第二個函式來擴充 Azure Functions 專案。
services: functions
author: kraigb
manager: barbkess
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 07f806dc58eb5aede65f0dca67e1bc59ce495a25
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172426"
---
# <a name="tutorial-add-a-second-python-function-to-azure-functions"></a>教學課程：將第二個 Python 函式新增至 Azure Functions

[上一個步驟：部署 Azure](tutorial-vs-code-serverless-python-05.md)

在您的第一個部署之後，您可以對程式碼進行變更，例如新增額外的函式，然後重新部署至相同的函式應用程式。

1. 在 [Azure：  函式] 總管中，選取 [建立函式]  命令，或從 [命令選擇區] 使用 [Azure Functions：  建立函式]。 為函式指定下列詳細資料：

    - 範本：HTTP 觸發程序
    - 名稱："DigitsOfPi"
    - 授權層級：匿名

1. 在 Visual Studio Code 中，檔案總管是具有您函式名稱的子資料夾，其中再次包含名為 *\_\_init\_\_.py*、*function.json* 和 *sample.dat* 的檔案。

1. 取代 *\_\_init\_\_.py* 的內容以符合下列程式碼，此程式碼會產生包含 PI 值的字串，直到 URL 中指定的位數 (此程式碼只會使用 URL 參數)

    ```python
    import logging

    import azure.functions as func

    """ Adapted from the second, shorter solution at http://www.codecodex.com/wiki/Calculate_digits_of_pi#Python
    """

    def pi_digits_Python(digits):
        scale = 10000
        maxarr = int((digits / 4) * 14)
        arrinit = 2000
        carry = 0
        arr = [arrinit] * (maxarr + 1)
        output = ""

        for i in range(maxarr, 1, -14):
            total = 0
            for j in range(i, 0, -1):
                total = (total * j) + (scale * arr[j])
                arr[j] = total % ((j * 2) - 1)
                total = total / ((j * 2) - 1)

            output += "%04d" % (carry + (total / scale))
            carry = total % scale

        return output;

    def main(req: func.HttpRequest) -> func.HttpResponse:
        logging.info('DigitsOfPi HTTP trigger function processed a request.')

        digits_param = req.params.get('digits')

        if digits_param is not None:
            try:
                digits = int(digits_param)
            except ValueError:
                digits = 10   # A default

            if digits > 0:
                digit_string = pi_digits_Python(digits)

                # Insert a decimal point in the return value
                return func.HttpResponse(digit_string[:1] + '.' + digit_string[1:])

        return func.HttpResponse(
             "Please pass the URL parameter ?digits= to specify a positive number of digits.",
             status_code=400
        )
    ```

1. 因為程式碼僅支援 HTTP GET，所以請修改 *function.json*，讓 `"methods"` 集合只包含 `"get"` (亦即，移除 `"post"`)。 整個檔案應如下所示：

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
            "get"
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

1. 按 F5 或選取 [偵錯]   > [啟動偵錯]  功能表命令來啟動偵錯工具。 [輸出]  視窗現在應該會在您的專案中顯示這兩個端點：

    ```output
    Http Functions:

            DigitsOfPi: [GET] http://localhost:7071/api/DigitsOfPi

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. 在瀏覽器中或從 Curl 中，對 `http://localhost:7071/api/DigitsOfPi?digits=125` 提出要求並觀察輸出。 (您可能會注意到，程式碼演算法並不完全精確，但我們會讓您自行改善！)完成時，請停止偵錯工具。

1. 重新部署程式碼，方法為使用 [Azure：函式]  總管中的 [部署至函式應用程式]  。 若出現提示，請選取先前建立的函式應用程式。

1. 一旦部署完成 (需要幾分鐘的時間！)，[輸出]  視窗就會顯示您可以重複測試的公用端點。

> [!div class="nextstepaction"]
> [我已新增第二個函式](tutorial-vs-code-serverless-python-07.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=06-second-function)
