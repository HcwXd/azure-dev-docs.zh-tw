---
title: 教學課程：將記錄從 Azure App Service 串流至 VS Code
description: 教學課程步驟 6：將應用程式記錄串流至 Visual Studio Code
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: a60c8fd0202e935960f14a9ab5570f86a78fab6e
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72278909"
---
# <a name="tutorial-stream-logs-from-azure-app-service-into-visual-studio-code"></a>教學課程：將 Azure App Service 中的記錄串流至 Visual Studio Code

[上一個步驟：部署應用程式](tutorial-deploy-app-service-on-linux-05.md)

使用下列程序，將 Azure App Service 中的記錄串流至 Visual Studio Code。

1. 在 Visual Studio Code 中，開啟 [Azure：  App Service] 總管、以滑鼠右鍵按一下 App Service，然後選取 [啟動串流記錄]  ：

   ![App Service 總管中的 [啟動串流記錄]](media/deploy-azure/start-streaming-logs-in-visual-studio-code.png)

1. 出現提示時，若要啟用檔案記錄，然後重新啟動 Web 應用程式，請選取 [是]  。 當應用程式重新啟動時，VS Code 中的 [輸出]  視窗會顯示進度。 啟用記錄是一次性程序。

1. 在啟用記錄之後，以滑鼠右鍵按一下 App Service，然後再次選取 [啟動串流記錄]  。 VS Code 中的 [輸出]  視窗即會顯示「正在啟動即時記錄串流」，而且記錄輸出會開始出現。 請嘗試在瀏覽器中重新整理 Web 應用程式，以產生更多記錄資訊。

1. 若要停止串流記錄 (不需停用記錄)，請以滑鼠右鍵按一下 [Azure：  App Service] 總管，然後選取 [停止串流記錄]  。

> [!div class="nextstepaction"]
> [我看到記錄](tutorial-deploy-app-service-on-linux-07.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=06-stream-logs)
