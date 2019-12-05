---
title: 在 Visual Studio Code 中進行變更之後，將容器重新部署至 Azure App Service
description: 教學課程步驟 5：重建和重新部署容器映像的簡單步驟。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 6ca29318b7dd5f1256d1b4503cf1ae9fc37ab111
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467114"
---
# <a name="make-changes-and-redeploy"></a>進行變更並重新部署

[上一步：部署應用程式映像](tutorial-vscode-docker-node-04.md)

因為您會無可避免地對應用程式進行變更，所以最後會重建並重新部署您的容器許多次。 幸運的是，程序很簡單：

1. 對您的應用程式進行變更，並在本機進行測試。

1. 在 Visual Studio Code 中，開啟 [命令選擇區]  (**F1**) 並執行 [Docker 映像:  建立映像] 以重建映像)。 如果您只變更應用程式碼，則組建應該只需要幾秒鐘的時間。

1. 若要將映像推送至登錄，請再次開啟 [命令選擇區]  (**F1**)，然後執行 [Docker 映像:  推送]，並選擇您剛建立的映像。 如同前面，因為對應用程式碼進行小幅變更，所以只需推送該層，而且程序通常會在幾秒內完成。

1. 在 [Azure：  App Service] 總管中，以滑鼠右鍵按一下適當的 App Service，然後選取 [重新啟動]  。 重新啟動 App Service 會自動從登錄中提取最新的容器映像。

1. 大約 15-20 秒之後，再次造訪 App Service URL 以檢查更新。

> [我看到變更](tutorial-vscode-docker-node-06.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=deploy-changes)
