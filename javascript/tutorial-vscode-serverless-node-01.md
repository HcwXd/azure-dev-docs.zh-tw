---
title: 從 Visual Studio Code 在 Node.js 中部署 Azure Functions
description: 教學課程第 1 部分：簡介和必要條件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 5dd41d710df629565699cff3bfd8e4bca7677087
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686071"
---
# <a name="deploy-azure-functions-from-visual-studio-code"></a>從 Visual Studio Code 部署 Azure Functions

在本教學課程中，您會使用 Visual Studio Code 和 Azure Functions 擴充功能來建立及部署以 JavaScript 撰寫的 Azure Functions 應用程式。 

## <a name="prerequisites"></a>必要條件

- [Azure 訂用帳戶](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure Functions 擴充功能](vscode:extension/ms-azuretools.vscode-azurefunctions)
- [Node.js 和 npm](https://nodejs.org/en/download)、Node.js 套件管理員。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azurefunctions">安裝 Azure Functions 擴充功能</a>

### <a name="azure-subscription"></a>Azure 訂用帳戶

如果您沒有 Azure 訂用帳戶，請[立即註冊](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)免費帳戶，即可獲得價值 200 美元的 Azure 點數，讓您可以試用各種服務組合。

## <a name="sign-in-to-azure"></a>登入 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="install-the-azure-functions-core-tools"></a>安裝 Azure Functions Core Tools

若要啟用本機偵錯，您必須安裝 [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools)，這可以直接在 Visual Studio Code 中完成。

1. 啟動 Visual Studio Code。

1. 開啟 [命令選擇區]  (**F1**)，輸入 [Azure Functions:  安裝或更新 Azure Functions Core Tools]，然後按 **Enter** 來執行命令。

1. 若要確認安裝，請在 VS Code 中選取功能表命令 [終端]   > [新增終端]  ，然後執行命令 `func`。 此命令應顯示如下所示的輸出 (以及使用量資訊)。

    ```output
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    ```

> [!div class="nextstepaction"]
> [我已安裝必要條件](tutorial-vscode-serverless-node-02.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=getting-started)
