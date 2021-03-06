---
title: 教學課程：使用 VS Code 在本機對 Azure Functions Python 程式碼進行偵錯
description: 教學課程步驟 4：在本機執行 VS Code 偵錯工具，以檢查您的 Python 程式碼。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: ffd5d433166c44edd8c021fd29cb7e43395df7ff
ms.sourcegitcommit: ac68fb174d606c7af2bfa79fe32b8ca7b73c86a1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946689"
---
# <a name="tutorial-debug-the-azure-functions-python-code-locally"></a>教學課程：在本機上針對 Azure Functions Python 程式碼進行偵錯

[上一個步驟：檢查程式碼檔案](tutorial-vs-code-serverless-python-03.md)

您可以在本機使用 Visual Studio Code 對 Azure Functions Python 程式碼進行偵錯。

1. 當您建立 Functions 專案時，Visual Studio Code 擴充功能也會在 `.vscode/launch.json` 中建立啟動設定，其中包含名為 **Attach to Python Functions** 的單一設定。 此設定表示您可以直接按 F5 或使用偵錯總管來啟動專案：

    ![偵錯總管用來啟動 Python 專案的組態](media/tutorial-vs-code-serverless-python/configuration-to-start-a-python-project-for-debugging.png)

1. 當您啟動偵錯工具時，終端機即會開啟，其中顯示來自 Azure Functions 的輸出，包括可用端點的摘要。 如果您已使用 "HttpExample" 以外的名稱，則您的 URL 可能會不同：

    ```output
    Http Functions:

            HttpExample: [GET,POST] http://localhost:7071/api/HttpExample
    ```

1. 在 Visual Studio Code [輸出]  視窗的 URL 上，使用 **Ctrl + 按一下滑鼠左鍵** 或 **Cmd + 按一下滑鼠左鍵**，將瀏覽器開啟至該位址，或啟動瀏覽器並貼上相同的 URL。 在任一情況下，端點都是 `api/<function_name>`，在此情況下，為 `api/HttpExample`。 不過，由於該 URL 未包含名稱參數，因此瀏覽器視窗應該只顯示「請在查詢字串上或在要求本文中傳遞名稱」，視程式碼中的該路徑適用於何者而定。

1. 現在，請嘗試新增要使用的名稱參數，例如 `http://localhost:7071/api/HttpExample?name=VS%20Code`，瀏覽器視窗應該會顯示訊息 "Hello Visual Studio Code!"，示範您已執行該程式碼路徑。

1. 若要在 JSON 要求本文中傳遞名稱值，您可以使用 Curl 這類的工具與 JSON 內嵌搭配：

    ```bash
    # Mac OS/Linux: modify the URL if you're using a different function name
    curl --header "Content-Type: application/json" --request POST \
        --data '{"name":"Visual Studio Code"}' http://localhost:7071/api/HttpExample
    ```

    ```ps
    # Windows (escaping on the quotes is necessary; also modify the URL
    # if you're using a different function name)
    curl --header "Content-Type: application/json" --request POST \
        --data {"""name""":"""Visual Studio Code"""} http://localhost:7071/api/HttpExample
    ```

    在 PowerShell 中，您還可以使用 [Invoke-WebRequest cmdlet](/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-6)。

    ---

    或者，建立像是 *data.json* 的檔案 (其中包含 `{"name":"Visual Studio Code"}`)，並使用 `curl --header "Content-Type: application/json" --request POST --data @data.json http://localhost:7071/api/HttpExample` 命令。

1. 若要測試偵錯函式，請在標明 `name = req.params.get('name')` 的那一行上設定中斷點，並再次對 URL 提出要求。 Visual Studio Code 偵錯工具應該會在該行停止，讓您可以檢查變數並逐步執行程式碼。 (如需基本偵錯工具的簡短逐步解說，請參閱 [Visual Studio Code 教學課程 - 設定和執行偵錯工具](https://code.visualstudio.com/docs/python/python-tutorial#configure-and-run-the-debugger))。

1. 當您滿意已在本機徹底測試函式時，請停止偵錯工具 (使用 [偵錯]   > [停止偵錯]  功能表命令，或偵錯工具列上的 [中斷連線]  命令)。

> [!div class="nextstepaction"]
> [我已在本機執行偵錯工具](tutorial-vs-code-serverless-python-05.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=04-test-debug)
