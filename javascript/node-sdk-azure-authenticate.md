---
title: 使用適用於 Node.js 的 Azure 管理模組進行驗證
description: 使用用來進入適用於 Node.js 之 Azure 管理模組的服務主體進行驗證
ms.topic: article
ms.date: 06/17/2017
ms.openlocfilehash: 76233fb6e6d15829783ff98b3af672abc7eba226
ms.sourcegitcommit: 5c65d22b5203b0c17806463d349a6ede93a99fa0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2019
ms.locfileid: "75010508"
---
# <a name="authenticate-with-the-azure-modules-for-nodejs"></a>使用適用於 Node.js 的 Azure 模組進行驗證

所有服務 API 在具現化時都需要透過 `credentials` 物件驗證。 透過 Azure SDK for Node.js 驗證並建立所需認證的方法有三種： 

- 基本驗證
- 互動式登入
- 服務主體驗證

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="basic-authentication"></a>基本驗證

若要以程式設計方式使用您的 Azure 帳戶的認證驗證，請使用 `loginWithUsernamePassword` 函式。 下列 JavaScript 程式碼片段說明如何使用儲存為環境變數的認證進行基本驗證。 

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.loginWithUsernamePassword(process.env.AZURE_USER, 
                                 process.env.AZURE_PASS, 
                                 (err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, 
                                                             '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="interactive-login"></a>互動式登入

互動式登入提供的連結和程式碼可讓使用者從瀏覽器驗證。 相同指令碼使用多個帳戶或偏好使用者介入時，請使用這個方法。

```javascript
const Azure = require('azure');
const MsRest = require('ms-rest-azure');

MsRest.interactiveLogin((err, credentials) => {
  if (err) throw err;

  let storageClient = Azure.createARMStorageManagementClient(credentials, '<azure-subscription-id>');

  // ..use the client instance to manage service resources.
});
```

## <a name="service-principal-authentication"></a>服務主體驗證

[互動式登入](#interactive-login)是最簡單的驗證方法。 不過，使用 Node.js SDK 時，建議您使用服務主體驗證，而非提供您的帳戶認證。 [使用 Node.js 建立 Azure 服務主體](./node-sdk-azure-authenticate-principal.md)主題說明用於建立 (和使用) 服務主體的各種技術。 
