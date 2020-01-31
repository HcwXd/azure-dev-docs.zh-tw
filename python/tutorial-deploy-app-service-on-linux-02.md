---
title: 教學課程：準備應用程式從 Visual Studio Code 將其部署至 Linux 上的 Azure App Service
description: 教學課程步驟 2：設定您的應用程式
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: d77da775a112185f7ccb81805272c5c70a2aecb3
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825772"
---
# <a name="tutorial-prepare-your-app-for-deployment-to-azure-app-service"></a>教學課程：準備應用程式以便部署至 Azure App Service

[上一個步驟：必要條件](tutorial-deploy-app-service-on-linux-01.md)

在本文中，您會準備一個應用程式以部署至本教學課程的 Azure App Service。 您可以使用現有的應用程式，或是建立或下載應用程式。

如果您已有想要使用的應用程式，請確定您有描述相依性的 *requirements.txt* 檔案，包括 Flask 或 Django 之類的架構。

如果您還沒有應用程式，請使用下列其中一個選項。 請務必確認應用程式在本機執行。

## <a name="minimal-flask-app"></a>最小 Flask 應用程式

本節說明在本逐步解說中使用的最小 Flask 應用程式。

1. 建立新的資料夾、在 VS Code 中開啟它，然後新增一個含有下列內容且名為 *hello.py* 的檔案。 應用程式物件會被刻意命名為 `myapp`，以示範如何在 App Service 的啟動命令中使用這些名稱，您在稍後將有所了解。

    ```python
    from flask import Flask
    myapp = Flask(__name__)

    @myapp.route("/")
    def hello():
        return "Hello Flask, on Azure App Service for Linux"
    ```

1. 建立一個含有下列內容且名為 *requirements.txt* 的檔案：

    ```text
    Flask==1.1.1
    ```

1. 遵循 [Flask 教學課程 - 建立 Flask 的專案環境](https://code.visualstudio.com/docs/python/tutorial-flask#create-a-project-environment-for-flask)中的指示來建立虛擬環境，其中會安裝 Flask，讓您可以在本機執行應用程式。

1. 若要執行此應用程式，請使用下列命令 (視您的作業系統而定)。 FLASK_APP 環境變數會告訴 Flask 哪裡可以找到應用程式物件。

    ```ps
    $env:FLASK_APP = "hello:myapp"
    ```

    ```bash
    export FLASK_APP=hello:myapp
    flask run
    ```

    ```cmd
    set FLASK_APP=hello:myapp
    flask run
    ```

    然後，您可以使用 URL `http://127.0.0.1:5000/`，在瀏覽器中開啟應用程式。

## <a name="vs-code-flask-tutorial-sample"></a>VS Code Flask 教學課程範例

下載或複製 [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial)，這是遵循 [Flask 教學課程](https://code.visualstudio.com/docs/python/tutorial-flask)的結果。

## <a name="vs-code-django-tutorial-sample"></a>VS Code Django 教學課程範例

下載或複製 [python-sample-vscode-django-tutorial](https://github.com/Microsoft/python-sample-vscode-django-tutorial)，這是遵循 [Django 教學課程](https://code.visualstudio.com/docs/python/tutorial-django)的結果。

如果您的 Django 應用程式使用類似此範例的本機 SQLite 資料庫，則您需要在存放庫中包含預先初始化和預先填入的 *db.sqlite3* 檔案複本。 原因是 Linux 的 App Service 目前沒有辦法在部署過程中執行 Django 的 `migrate` 命令，因此您必須部署預先製作的資料庫。 就算如此，資料庫實際上還是唯讀的；寫入至資料庫也會造成錯誤。

在任何情況下，最佳選項是使用獨立於應用程式碼以外部署和初始化的個別資料庫。

> [!div class="nextstepaction"]
> [我已準備好應用程式](tutorial-deploy-app-service-on-linux-03.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=02-prepare-app)
