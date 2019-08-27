---
title: 將 Node.js Web 應用程式部署至 Azure
description: 開始使用適用於 Web 應用程式的 Azure App Service 和其他裝載選項，包括漸進式 Web 應用程式 (PWA)
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.openlocfilehash: c58cd1ed3480fae88d9839b44f0f4062c825a068
ms.sourcegitcommit: f519a1ee8017850b2fa37049af3bac1ea5ca5516
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2019
ms.locfileid: "69892342"
---
# <a name="how-to-deploy-web-apps-to-azure"></a>如何將 Web 應用程式部署至 Azure

在 Azure 上，您有數個選項可裝載 Web 應用程式：

- Web 應用程式的最佳裝載選項是 Azure App Service，這是一個平台即服務 (PaaS) 供應項目。 若要開始使用，請嘗試下列任何一項資源：

  - [在 Azure 中建立 Node.js Web 應用程式](/azure/app-service/app-service-web-get-started-nodejs)
  - [嘗試 Azure App Service - 從範本建立快速應用程式](https://code.visualstudio.com/tryappservice/?utm_source=msftdocs&utm_medium=microsoft&utm_campaign=tryappservice)
  - [使用 Azure App Service 來裝載 Web 應用程式 - 學習模組](/learn/modules/host-a-web-app-with-azure-app-service/index)
  - [在 Azure 中建置 Node.js 和 MongoDB 應用程式](/azure/app-service/app-service-web-tutorial-nodejs-mongodb-app)
  - [App Service 範例](/samples/browse/?languages=javascript%2Cnodejs&products=azure-app-service)

- 您可以建置自己的容器，並使用 Azure Container Registry 和 Azure Kubernetes Service 將其部署至 Azure。 如需詳細資訊，請參閱[如何將 Node.js 容器部署至 Azure](node-howto-deploy-containers.md)。

- 如果您想要主要使用無伺服器程式碼，請參閱[如何在 Azure 上撰寫無伺服器 Node.js 程式碼](node-howto-write-serverless-code.md)。

- 如需建立 JAMstack (靜態) 網站的詳細資訊，請參閱[如何使用 Azure 建立 JAMstack (靜態網站) Web 應用程式](node-howto-create-static-site-jamstack.md)。

- 如果您想要控制基礎結構，您可以直接使用虛擬機器。 若要開始，請遵循 Microsoft Learn 上的[使用 Azure 虛擬機器部署網站](/learn/paths/deploy-a-website-with-azure-virtual-machines/)路徑。

如需不同裝載選項的完整概觀，請參閱 [Azure 計算服務的決策樹](/azure/architecture/guide/technology-choices/compute-decision-tree)和 Microsoft Learn 上的[核心雲端服務 - Azure 計算選項](/learn/modules/intro-to-azure-compute/)模組。