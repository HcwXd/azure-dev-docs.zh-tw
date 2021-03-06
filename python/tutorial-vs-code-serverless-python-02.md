---
title: 教學課程：使用 VS Code 建立 Azure Functions 的 Python 函式
description: 教學課程步驟 2：示範如何使用 VS Code 的 Azure Functions 擴充功能。
ms.topic: conceptual
ms.date: 09/02/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 49ab6b150f14268b6d52ac48524f66e6e520e547
ms.sourcegitcommit: 68a4044b9fa3291c9e7e2f68ae0049328f9c01bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2019
ms.locfileid: "74992524"
---
# <a name="tutorial-create-a-python-function-for-azure-functions"></a>教學課程：建立適用於 Azure Functions 的 Python 函式

[上一個步驟：必要條件](tutorial-vs-code-serverless-python-01.md)

在本文中，您將使用 Visual Studio Code 建立 Azure Functions 的 Python 函式。 Azure Functions 的程式碼是在 Functions _專案_內進行管理，您會先建立此專案，然後再建立程式碼。

1. 在 [Azure：  函式] 總管 (使用左側的 Azure 圖示開啟) 中，選取 [新增專案]  命令圖示，或開啟 [命令選擇區] (F1)，然後選取 [Azure Functions：**建立新專案]** 。

    ![在 Azure Functions 總管中建立新的專案](media/tutorial-vs-code-serverless-python/create-a-new-project-in-azure-functions-explorer.png)

1. 在隨後的提示中：

    | Prompt | 值 | 說明 |
    | --- | --- | --- |
    | 指定專案的資料夾 | 目前開啟的資料夾 | 要在其中建立專案的資料夾。 您可能想要在子資料夾中建立專案。 |
    | 為您的函式應用程式專案選取語言 | **Python** | 用於函式的語言，可決定用於程式碼的範本。 |
    | 選取您專案第一個函式的範本 | **HTTP 觸發程序** | 每當對函式端點提出 HTTP 要求，就會執行一個使用 HTTP 觸發程序的函式。 (Azure Functions 有各種不同的觸發程序。 若要深入了解，請參閱[我可以用 Functions 來做什麼？](/azure/azure-functions/functions-overview#what-can-i-do-with-functions)。) |
    | 提供函式名稱 | HttpExample | 此名稱用於子資料夾，其中包含函式程式碼以及設定資料，而且也會定義 HTTP 端點的名稱。 使用 "HttpExample"，而不是接受預設 "HTTPTrigger"，以區別函式本身與觸發程序。 |
    | 授權層級 | **匿名** | 匿名授權可讓任何人都能公開存取函式。 |
    | 選取您要如何開啟專案 | **在目前視窗中開啟** | 在目前 Visual Studio Code 視窗中開啟專案。 |

    > [!NOTE]
    > 如果您已安裝 Python 3.6 和 3.7，Visual Studio Code 依預設會對 Azure Functions 專案使用 Python 3.6。 若要使用目前的 Python 3.7，請先建立並啟用 Python 3.7 環境，然後從終端機使用 `func init` 命令。 接著，使用 `code .` 命令，從該資料夾重新啟動 Visual Studio Code。

1. 不久之後，會出現一則訊息，指出已建立新專案。 在 [總管]  中，有針對函式建立的子資料夾，而且 Visual Studio Code 會開啟 *\_\_init\_\_.py* 檔案，其中包含預設函式程式碼：

    ![在 Azure Functions 中建立新 Python 專案的結果](media/tutorial-vs-code-serverless-python/display-results-of-new-python-project-in-azure-functions.png)

    > [!NOTE]
    > 如果 Visual Studio Code 告訴您，在開啟 *\_\_init\_\_.py* 時未選取 Python 解譯器，請開啟 [命令選擇區] (**F1**)、選取 [Python：  選取解譯器] 命令，然後在本機 `.env` 資料夾 (已建立為專案的一部分) 中選取虛擬環境。 此環境必須特別以 Python 3.6 x 為基礎，如前一篇文章的[必要條件](tutorial-vs-code-serverless-python-01.md#prerequisites)中之下所述。
    >
    > ![選取以 Python 專案建立的虛擬環境](media/tutorial-vs-code-serverless-python/select-virtual-environment-created-with-the-python-project.png)

> [!TIP]
> 每當您想要在相同的專案中建立另一個函式時，請使用 [Azure：函式]  總管中的 [建立函式]  命令，或開啟 [命令選擇區] (**F1**)，然後選取 [Azure Functions：**建立函式]** 命令。 這兩個命令都會提示您輸入函式名稱 (這是端點的名稱)，然後使用預設檔案建立子資料夾。
>
> ![在 Azure Functions 總管中使用 [新增函式] 建立函式](media/tutorial-vs-code-serverless-python/create-new-functions-in-azure-functions-explorer.png)

> [!div class="nextstepaction"]
> [我已建立函式](tutorial-vs-code-serverless-python-03.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-functions-python&step=02-create-function)
