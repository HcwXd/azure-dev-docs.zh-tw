---
title: 使用 Node.js、API 和標記在 Azure 上建置靜態網站
description: 如何使用 Azure 來建置 JAMstack 應用程式 (JavaScript、API 和標記)
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/20/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019
ms.openlocfilehash: dc1d376be0f57d7d79a7a67d43dca49c30163c90
ms.sourcegitcommit: 52fa18873a6a8dc7f28c063cca0175bae2720b2a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/09/2019
ms.locfileid: "70808461"
---
# <a name="how-to-build-jamstack-static-site-web-apps-with-azure"></a>如何使用 Azure 建置 JAMstack (靜態網站) Web 應用程式

使用 JavaScript  前端、API  (以無伺服器程式碼建置的第三方或自訂 API) 的組合，以及可作為靜態頁面的樣板化標記  (HTML 和 CSS)，可以有效率地建置及維護絕佳的 Web 應用程式。 使用這種組合 (也稱為 JAMstack)，您可以避免撰寫複雜的後端程式碼來提供網頁。 相反地，系統只會服務靜態頁面 (HTML、CSS 和 JavaScript)，而這些頁面會在您的 API 上呼叫以進行伺服器端工作。 由於您可以利用自動調整無伺服器技術來撰寫這些 API，因此您可以完全避免使用一般永遠開啟伺服器或 Web 主機的成本和安全性考量。 (如需詳細資訊，請參閱 [jamstack.org](https://jamstack.org/)。)

若要在 Azure 上實作靜態/JAMstack 網站，您可以運用各種工具和服務：

- 視需要設定資料庫。
- 在 Azure Functions 中實作無伺服器 API 程式碼。 這些 API 通常會使用資料庫。
- 選擇您想要用於前端開發的任何程式庫，例如 Angular。 接著，您可以將這些靜態 HTML、CSS 和 JavaScript 檔案上傳到 Azure Blob 儲存體，以提供內建的 Web 伺服器。
- 建立反向 Proxy，讓您的所有流量通過一個 URL 網域。

您可以觀看 //build 2019 工作階段程序，[使用 JavaScript、Visual Studio Code 和 Azure 的高生產力前端開發](https://mybuild.techcommunity.microsoft.com/sessions/77038?source=sessions#top-anchor)的示範。

> [!VIDEO https://medius.studios.ms/Embed/Video-nc/B19-BRK3021?latestplayer=true]

如需逐步教學課程，請參閱 Visual Studio Code 文件中的[將靜態網站部署至 Azure](https://code.visualstudio.com/tutorials/static-website/getting-started)。

下列文章也會說明進一步的詳細資料：

- **資料庫**：您可以使用任何您想要的資料庫，例如 Azure 上的不同資料庫服務，如同[如何在 Node.js 應用程式中整合 Azure 資料庫](node-howto-integrate-databases.md)中所述。
  
- **無伺服器 API**：

  - 首先，[使用 Visual Studio Code 建立您的第一個函式](/azure/azure-functions/functions-create-first-function-vs-code)，這會為您在 Visual Studio Code 內容中導入 Azure Functions，可簡化許多詳細資料。
  - 當您完成本文時，您會有一個 Azure Functions 專案 (資料夾)，其中包含針對函式命名的子資料夾，它與 HTTP 端點相同。 該函式資料夾包含具有程式碼的 index.js  檔案。
  - 您可以視需要修改該函式，並在專案中新增更多函式，然後再次將其部署至 Azure，以供公開使用。
  - 如需無伺服器開發的其他資源，請參閱[如何在 Azure 上撰寫無伺服器 Node.js 程式碼](node-howto-write-serverless-code.md)

- **將您的前端部署到 Azure 儲存體**：使用您的 API 時，您現在可以撰寫前端程式碼，使用您喜歡的任何架構來使用這些 API。 當您準備好時，請遵循文章，[教學課程：在 Blob 儲存體上裝載靜態網站](/azure/storage/blobs/storage-blob-static-website-host)，將這些檔案上傳至 Azure，並開啟靜態網站裝載。

- **建立反向 Proxy**：反向 Proxy (如[使用 Azure Functions Proxy](/azure/azure-functions/functions-proxies) 中所述) 可讓您輕鬆地將特定要求導向至不同的 URL。 在此情況下，您會想要將前端檔案的要求導向至 Azure 儲存體 URL，您在其中部署這些檔案，並將 API 要求導向至 Azure Functions URL。

  - 若要建立這些 Proxy，請在 Functions 專案中編輯 proxy.json  檔案，使其如下所示，並且以您的 URL 取代 `<storage_url>` 和 `<api_url>`：
  
    ```json
    {
      "$schema": "http://json.schemastore.org/proxies",
      "proxies": {
        "Static frontend on Azure Storage": {
          "matchCondition": {
            "route": "/{*restOfPath}"
          },
          "backendUri": "<storage_url>/{restOfPath}"
        },
        "Azure Functions API": {
          "matchCondition": {
            "route": "/api/{*restOfPath}"
          },
          "backendUri": "<api_url>/api/{restOfPath}"
        }
      }
    }
    ```
