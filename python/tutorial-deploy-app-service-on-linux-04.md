---
title: 教學課程：在 Linux 上的 Azure App Service 設定 Python 應用程式的自訂啟動檔案
description: 教學課程步驟 4：指示 App Service 如何啟動 Web 應用程式。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: e18d58a5caf18103063fabfa3101988399bbb722
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467057"
---
# <a name="tutorial-configure-a-custom-startup-file-for-python-apps-on-azure-app-service"></a>教學課程：在 Azure App Service 上設定 Python 應用程式的自訂啟動檔案

[上一個步驟：建立 App Service](tutorial-deploy-app-service-on-linux-03.md)

本文說明如何在 Azure App Service 上設定 Python 應用程式的自訂啟動檔案。

根據您建構應用程式的方式，您可能需要為應用程式建立自訂啟動命令檔，如 Azure 文件中的[設定適用於 Azure App Service 的 Linux Python 應用程式](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)所述。

自訂啟動命令的特定使用案例如下：

- 您有一個 **Flask** 應用程式，其啟動檔案和應用程式物件分別命名為 **application.py** 和 `app` *以外*的名稱。 換句話說，除非您在專案的根資料夾中有 *application.py*，*而且* Flask 應用程式物件名為 `app`，否則您需要一個自訂啟動命令。
- 您想要使用超出預設值的其他引數來啟動 Gunicorn Web 伺服器，亦即 `--bind=0.0.0.0 --timeout 600`。

## <a name="create-a-startup-file"></a>建立啟動檔案

如果您需要一個自訂啟動檔案，請使用下列步驟：

1. 在您的專案中建立名為 *startup.txt* (或另一個您選擇的名稱) 的檔案，其中包含您的啟動命令。 如需 Flask，請參閱下一節中的 [Flask 啟動命令](#flask-startup-commands)。 Django 應用程式通常不需要自訂。

1. 將檔案認可至您的程式碼存放庫，讓它可以與應用程式的其餘部分一起部署。

1. 在 [Azure：  App Service] 總管中，展開 App Service、以滑鼠右鍵按一下 [應用程式設定]  ，然後選取 [在入口網站中開啟]  ：

    ![使用 App Service 總管在入口網站中開啟應用程式設定](media/deploy-azure/open-application-settings-in-portal-for-app-service.png)

1. 在 Azure 入口網站中，視需要登入；然後在 [設定]  頁面上，選取 [一般設定]  、在 [堆疊設定]   > [啟動命令]  下輸入啟動檔案的名稱 (例如 *startup.txt*)，然後選取 [儲存]  。

    ![在 Azure 入口網站中設定啟動命令檔案名稱](media/deploy-azure/enter-startup-file-for-app-service-in-the-azure-portal.png)

    > [!NOTE]
    > 您也可以將啟動命令直接放在 Azure 入口網站上的 [啟動命令]  欄位中，而不是使用啟動命令檔案。 不過，一般最好使用檔案，因為它會在您的存放庫中保留這一項設定，讓您可以同時在其中稽核變更並重新部署到不同的 App Service 實例。

1. 當您儲存變更時，App Service 會重新啟動。 不過，由於您仍未部署應用程式碼，此時造訪網站會顯示「應用程式錯誤」。 此訊息指出 Gunicorn 伺服器已啟動，但找不到應用程式，因此未對 HTTP 要求做出任何回應。 您會在下一個步驟中部署應用程式碼。

## <a name="django-startup-commands"></a>Django 啟動命令

根據預設，App Service 會自動尋找包含 *wsgi.py* 檔案的資料夾，並使用下列命令啟動 Gunicorn：

```bash
# <module> is the path to the folder that contains wsgi.py
gunicorn --bind=0.0.0.0 --timeout 600 <module>.wsgi
```

如果您想要變更任何 Gunicorn 引數，例如使用 `--timeout 1200`，則請利用那些修改來建立命令檔。

## <a name="flask-startup-commands"></a>Flask 啟動命令

根據預設，Linux 容器上的 App Service 假設 Flask 應用程式的啟動檔案名為 *application.py*，並位於應用程式的根資料夾中。 它會進一步假設在該檔案內定義的 Flask 應用程式物件名為 `app`。 如果您的應用程式不完全以這種方式建構，則您的自訂啟動命令必須識別應用程式物件的位置：

1. **不同的檔案名稱和/或應用程式物件名稱**：例如，如果應用程式的啟動檔案是 *hello.py*，而應用程式物件名為 `myapp`，則啟動命令如下所示：

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 hello:myapp
    ```

1. **啟動檔位於子資料夾中**：例如，如果啟動檔案是 *myapp/.py*，而應用程式物件是 `app`，則請使用 Gunicorn 的 `--chdir` 引數來指定資料夾，然後如往常命名啟動檔案和應用程式物件：

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 --chdir myapp website:app
    ```

1. **啟動檔案位於模組內**：在 [python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) 程式碼中，*webapp.py* 啟動檔案包含在資料夾 *hello_app* 內，此資料夾本身就是具有 *\_\_init\_\_.py* 檔案的模組。 應用程式物件名為 `app`，並定義在 *\_\_init\_\_.py* 中，而且 *webapp.py* 會使用相對匯入。 由於這種排列，將 Gunicorn 指向 `webapp:app` 會產生「已在非封裝中嘗試相對匯入」錯誤，而且應用程式無法啟動。

    在此情況下，請建立簡單的填充碼檔案，從模組匯入應用程式物件，然後讓 Gunicorn 使用填充碼啟動應用程式。 例如，[python-sample-vscode-flask-tutorial](https://github.com/Microsoft/python-sample-vscode-flask-tutorial) 程式碼包含 *startup.py* 與下列內容：

    ```python
    # startup.py shim
    from hello_app.webapp import app
    ```

    然後，啟動命令如下所示：

    ```text
    gunicorn --bind=0.0.0.0 --timeout 600 startup:app
    ```

> [!div class="nextstepaction"]
> [我已設定啟動檔案](tutorial-deploy-app-service-on-linux-05.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=04-startup-command)
