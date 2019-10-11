---
title: 從 Visual Studio Code 建立 Azure Functions 應用程式
description: 教學課程第 2 部分：建立 Azure Functions 應用程式
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 190e574e9614eb11d913c8b26904f7b80dab35fc
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686000"
---
# <a name="create-the-local-functions-app"></a>建立本機 Functions 應用程式

[上一步：簡介和必要條件](tutorial-vscode-serverless-node-01.md)

在此步驟中，您會建立本機 Azure Functions 應用程式，其中包含使用 HTTP 觸發程序的函式。 Azure Functions 應用程式可以包含許多具有不同觸發程序的 Functions。 HTTP 觸發程序會特別處理傳入的 HTTP 流量。

1. 從終端或命令提示字元，在專案的適當資料夾內執行 Visual Studio Code：

    ```bash
    # Create and navigate to a project folder

    # Run VS Code in that folder
    code .
    ```

1. 在 VS Code 中，選取 Azure 標誌以開啟 **Azure Functions** 總管，然後選取 [建立專案]  命令：

    ![在 VS Code 中建立本機函式應用程式](media/functions-extension/create-function-app-project.png)

1. 在前兩個提示中，選取目前的資料夾，然後針對語言選取 [JavaScript]  。

1. 出現 [為您專案的第一個函式選取範本]  提示時，選取 [HTTP 觸發程序]  ：

    ![選取 Function 的觸發程序](media/functions-extension/create-function-choose-template.png)

1. 出現 [提供函式名稱]  提示時，輸入 **HttpExample**。 (請避免使用預設的 "HttpTrigger" 名稱，因為它與觸發程序相同，這可能會造成混淆。)

    ![輸入函式名稱](media/functions-extension/create-function-name.png)

1. 出現 [授權等級]  提示時，選取 [匿名]  ：

    ![輸入函式名稱](media/functions-extension/create-function-anonymous-auth.png)

1. 一會兒之後，VS Code 會完成專案的建立。 您的函式有一個名為 *HttpExample*的資料夾，其中有三個檔案：

    | 檔案名稱 | 說明 |
    | --- | --- |
    | *index.js* |  可回應 HTTP 要求的原始程式碼。 |
    | *functions.json* | HTTP 觸發程序的[繫結設定](/azure/azure-functions/functions-triggers-bindings)。 |
    | *sample.dat* | 預留位置資料檔案，示範您可在資料夾中擁有其他檔案。 您可視需要刪除此檔案，因為本教學課程不會用到它。 |

    ![建立函式應用程式的結果](media/functions-extension/create-function-app-results.png)

> [!div class="nextstepaction"]
> [我建立 Functions 應用程式](tutorial-vscode-serverless-node-03.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=create-app)
