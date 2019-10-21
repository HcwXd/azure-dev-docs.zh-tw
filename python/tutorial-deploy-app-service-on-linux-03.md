---
title: 教學課程：從 Visual Studio Code 建立 App Service
description: 教學課程步驟 3：從 VS Code 擴充功能建立 App Service。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.custom: seo-python-october2019
ms.openlocfilehash: 947a474f9bd6efa7fdb3c0a371f140623252aa25
ms.sourcegitcommit: 6012460ad8d6ff112226b8f9ea6da397ef77712d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72279058"
---
# <a name="tutorial-create-the-app-service-from-visual-studio-code"></a>教學課程：從 Visual Studio Code 建立 App Service

[上一個步驟：準備您的應用程式](tutorial-deploy-app-service-on-linux-02.md)

在此步驟中，請建立您要在其中部署應用程式的 Azure App Service 執行個體。

您會在部署程式碼之前執行此步驟，以便如有需要可在下一個步驟中設定自訂啟動檔案。

1. 在 [Azure：  App Service] 總管中，選取 **+** 命令來建立新的 App Service，或開啟 [命令選擇區] (**F1**)， 然後選取 [Azure App Service：  建立新的 Web 應用程式]。 (在 App Service 術語中，「Web 應用程式」是 Web 應用程式碼的**主機**，而不是應用程式碼本身。)

    ![App Service 總管中的 [建立新的 App Service]](media/deploy-azure/create-new-app-service-in-app-service-explorer.png)

1. 在隨後的提示中：

    - 輸入您應用程式的名稱，其在 App Service 上必須是全域唯一的；通常，您會使用您的名稱或公司名稱，後面接著應用程式名稱。
    - 選取 **Python 3.7** 做為執行階段。

1. 當出現訊息指出已建立新的 App Service 時，請選取 [檢視輸出]  以切換至 VS Code 中的 [輸出]  視窗。 輸出會顯示已建立的 Azure 資源群組名稱和 App Service 方案名稱，以及 App Service 的 URL。

    ![App Service 的 URL、資源群組和 App Service 方案](media/deploy-azure/url-for-your-new-app-service-and-resource-group-and-plan.png)

1. 若要確認 App Service 是否正常運作，請在 [Azure：  App Service] 總管中展開您的訂用帳戶、以滑鼠右鍵按一下適當的 App Service 名稱，然後選取 [瀏覽網站]  ：

    ![在 App Service 總管中的 App Service 上瀏覽網站命令](media/deploy-azure/select-command-to-browse-website-in-app-service.png)

1. 因為您尚未將自己的程式碼部署至 App Service (您會在下一個步驟中執行此部署)，所以只會顯示預設應用程式：

    ![Linux 上 App Service 上的預設 Python 應用程式](media/deploy-azure/default-python-app-on-app-service-on-linux.png)

## <a name="optional-upload-an-environment-variable-definitions-file"></a>(選用) 上傳環境變數定義檔案

如果您有環境變數定義檔，也可以使用該檔案來設定 App Service 環境。 (若要深入了解這類檔案 (通常具有 *.env* 副檔名)，請參閱 [Visual Studio Code - Python 環境](https://code.visualstudio.com/docs/python/environments#environment-variable-definitions-file)。)

1. 在 [Azure：  App Service] 總管中，展開所需 App Service 的節點，然後以滑鼠右鍵按一下 [應用程式設定]  節點，然後選取 [上傳本機設定]  。

1. VS Code 會提示您輸入 *.env* 檔案的位置，然後將它上傳至 App Service。

1. 一旦上傳完成，您就可以展開 [應用程式設定]  來查看個別值。 您也可以瀏覽至 App Service，然後選取 [設定]  ，在 Azure 入口網站上檢視它們。

1. 如果您直接在 Azure 入口網站上建立設定，則可以用滑鼠右鍵按一下 [應用程式設定]  節點，然後選取 [下載遠端設定]  ，將它們儲存在定義檔中。 此程序可確定您的存放庫中有這些設定，而不只是在入口網站上才有。

> [!div class="nextstepaction"]
> [我已建立 App Service](tutorial-deploy-app-service-on-linux-04.md)

[我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=03-create-app-service)
