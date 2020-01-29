---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 4228d1db44ac16da1c05fe97d20a3491cc9c5f51
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288567"
---
### <a name="inventory-secrets"></a>清查祕密

#### <a name="passwords-and-secure-strings"></a>密碼和安全字串

檢查實際執行伺服器上的所有屬性和設定檔是否有任何秘密字串和密碼。 務必檢查 $CATALINA_BASE/conf. 中的 *server.xml* 和 *context.xml*。 您也可以在應用程式內找到包含密碼或認證的設定檔。 其中可能包括 *META-INF/context.xml*，若為 Spring Boot 應用程式，則包括 *application.properties* 或 *application.yml* 檔案。

#### <a name="certificates"></a>憑證

記載所有用於公用 SSL 端點的憑證。 您可以執行下列命令來檢視實際執行伺服器上的所有憑證：

```bash
keytool -list -v -keystore <path to keystore>
```
