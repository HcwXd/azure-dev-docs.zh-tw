---
title: 從 Visual Studio Code 建立 Node.js 應用程式的容器映像
description: 教學課程第 3 部分：建立 Node.js 應用程式映像
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 1b79f84bd69853578796b4485ca669be98f41006
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686100"
---
# <a name="create-your-nodejs-application-image"></a>建立您的 Node.js 應用程式映像

[上一步：使用容器登錄](tutorial-vscode-docker-node-02.md)

在此步驟中，您會使用 Visual Studio Code 中的 Docker 擴充功能來新增所需的檔案，以建立您應用程式的映像、建置映像，並將其推送到登錄。

如果您還沒有此逐步解說適用的應用程式，請使用 [Visual Studio Code Node.js 教學課程](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)中的應用程式。

## <a name="add-docker-files"></a>新增 Docker 檔案

1. 在 Visual Studio Code 中開啟 [命令選擇區]  (**F1**)，鍵入 `add docker files to workspace`，然後選取 [Docker 映像:  將 Docker files 新增至工作區] 命令。

1. 出現提示時，針對應用程式類型選取 [Node.js]  ，然後選取您的應用程式接聽所在的連接埠。

1. 此命令會建立 `Dockerfile` (連同 Docker Compose 的一些組態檔) 和 `.dockerignore`。

    > [!TIP]
    > VS Code 對 Docker 檔案有絕佳的支援。 請參閱 VS Code 文件中的[使用 Docker](https://code.visualstudio.com/docs/azure/docker)，以了解豐富的語言功能，例如智慧型建議、完成和錯誤偵測。

## <a name="build-a-docker-image"></a>建置 Docker 映像

`Dockerfile` 描述您應用程式的環境，包括來源檔案的位置，以及可在容器內啟動應用程式的命令。

> [!TIP]
> 容器與影像：容器是影像的執行個體。

1. 開啟 [命令選擇區]  (**F1**) 並執行 [Docker 映像:  建置映像] 以建置映像)。

1. 出現提示時，選擇剛建立的 `Dockerfile`，然後提供映像名稱。 此名稱必須包含您的目標登錄或 Docker Hub 帳戶：

    `[registry or username]/[image name]:[tag]`

    在本教學課程中 (使用 Azure Container Registry)，映像名稱如下所示：

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    如果您使用 Docker Hub，請使用您的 Docker Hub 使用者名稱。 例如︰

    `fiveisprime/myexpressapp:latest`

1. 完成後，Visual Studio Code 的 [終端]  面板隨即開啟，以執行 `docker build` 命令。 輸出也會顯示構成應用程式環境的每個步驟或圖層。

1. 建置後，映像就會出現在 **DOCKER** 總管的 [映像]  之下。

    ![Visual Studio Code 中的 Docker 映像清單](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>將映像推送至登錄

1. 在 Visual Studio Code 中，開啟 [命令選擇區]  (**F1**) 並執行 [Docker 映像:  推送]，然後選擇您剛建立的映像。 [終端]  面板會顯示用於此作業的 `docker push` 命令。

1. 完成後，展開 Docker 擴充功能瀏覽器中的 [映像]  節點，以查看您的映像。

    ![Azure Container Registry 中顯示的已推送映像](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [我已建立應用程式的映像](tutorial-vscode-docker-node-04.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
