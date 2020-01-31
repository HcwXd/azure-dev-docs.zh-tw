---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: affabacec95b8f1c4c7ea654ff9a765056220c76
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825810"
---
### <a name="inventory-external-resources"></a>清查外部資源

外部資源 (例如資料來源、JMS 訊息代理程式和其他資源) 會透過 JAVA 命名和目錄介面 (JNDI) 插入。 某些這類資源可能需要進行遷移或重新設定。

#### <a name="inside-your-application"></a>在應用程式內

檢查 *META-INF/context.xml* 檔案。 尋找 `<Context>` 元素內的 `<Resource>` 元素。

#### <a name="on-the-application-servers"></a>在應用程式伺服器上

檢查 *$CATALINA_BASE/conf/context.xml* 和 *$CATALINA_BASE/conf/server.xml* 檔案，以及 *$CATALINA_BASE/conf/[engine-name]/[host-name]* 目錄中找到的 *.xml* 檔案。

在 *context.xml* 檔案中，JNDI 資源將由最上層 `<Context>` 元素內的 `<Resource>` 元素描述。

在 *server.xml* 檔案中，JNDI 資源將由 `<GlobalNamingResources>` 元素內的 `<Resource>` 元素描述。

#### <a name="datasources"></a>資料來源

資料來源是 `type` 屬性設為 `javax.sql.DataSource` 的 JNDI 資源。 針對每個資料來源，記載下列資訊：

* 資料來源名稱是什麼？
* 連線集區設定是什麼？
* 何處可以找到 JDBC 驅動程式 JAR 檔案？

#### <a name="all-other-external-resources"></a>所有其他外部資源

在本指南中記載每個可能的外部相依性並不可行。 驗證遷移後您可以滿足應用程式的每個外部相依性，這是您小組的責任。
