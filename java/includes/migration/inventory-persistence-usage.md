---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 23289c7dc4b608c6fe8bb75af479ea877a2000d9
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288617"
---
### <a name="inventory-persistence-usage"></a>清查持續性使用量

每當使用應用程式伺服器上的檔案系統時，都必須重新設定，或在罕見的情況下，還需要進行架構變更。 檔案系統可由 Tomcat 模組或您的應用程式碼使用。 您可以識別下列部分或全部案例。

#### <a name="read-only-static-content"></a>唯讀靜態內容

如果您的應用程式目前提供靜態內容 (例如，透過 Apache 整合)，您將需要該靜態內容的替代位置。 您可能想要考慮將靜態內容移至 Azure Blob 儲存體，並新增 Azure CDN，以在全球進行閃電般快速下載。 如需詳細資訊，請參閱 [Azure 儲存體中的 靜態網站裝載](/azure/storage/blobs/storage-blob-static-website)和[啟用儲存體帳戶的 Azure CDN](/azure/cdn/cdn-create-a-storage-account-with-cdn#enable-azure-cdn-for-the-storage-account)。

#### <a name="dynamically-published-static-content"></a>動態發佈的靜態內容

如果您的應用程式允許您應用程式上傳/產生的靜態內容，但在建立後無法變動，則您可以使用上述的 Azure Blob 儲存體和 Azure 函數，搭配 Azure 函數來處理上傳和 CDN 重新整理。 我們已在[使用 Azure Functions 上傳和透過 CDN 預先載入靜態內容](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)中提供範例實作供您使用。
