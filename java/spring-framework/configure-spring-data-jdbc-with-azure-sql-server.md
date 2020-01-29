---
title: 如何搭配使用 Spring Data JDBC 和 Azure SQL Database
description: 了解如何搭配使用 Spring Data JDBC 和 Azure SQL Database。
services: sql-database
documentationcenter: java
ms.date: 12/19/2018
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: b75db0f3cff02b5f7ead90265a170fc63405dbb6
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2020
ms.locfileid: "76283331"
---
# <a name="how-to-use-spring-data-jdbc-with-azure-sql-database"></a>如何搭配使用 Spring Data JDBC 和 Azure SQL Database

## <a name="overview"></a>概觀

本文示範如何建立使用 [Spring Data] 的應用程式範例，以在 [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 中使用 [Java 資料庫連線 (JDBC)](https://docs.oracle.com/javase/8/docs/technotes/guides/jdbc/) 儲存和擷取資訊。

## <a name="prerequisites"></a>Prerequisites

請務必具備下列必要條件，以便本文中說明的步驟：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) \(英文\) 3.0 版或更新版本。
* [Curl](https://curl.haxx.se/) 或類似的 HTTP 公用程式，以便測試功能。
* [Git](https://git-scm.com/downloads) 用戶端。

## <a name="create-an-azure-sql-database"></a>建立 Azure SQL Database

### <a name="create-a-sql-database-server-using-the-azure-portal"></a>使用 Azure 入口網站建立 SQL 資料庫伺服器

> [!NOTE]
> 
> 您可以在[在 Azure 入口網站中建立 Azure SQL 資料庫](/azure/sql-database/sql-database-get-started-portal)中，閱讀更多關於如何建立 Azure SQL Database 的詳細資訊。

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [+建立資源]  、[資料庫]  ，然後按一下 [SQL Database]  。

   ![建立 SQL 資料庫][SQL01]

1. 指定下列資訊：

   * **資料庫名稱**：為 SQL Database 選擇唯一的名稱；您會在稍後將會指定的 SQL 伺服器中建立此名稱。
   * 訂用帳戶  ：指定您要使用的 Azure 訂用帳戶。
   * **資源群組**：指定是要建立新的資源群組，還是選擇現有的資源群組。
   * **選取來源**：在此教學課程中，選取 `Blank database` 以建立新的資料庫。

   ![指定 SQL Database 屬性][SQL02]
   
1. 按一下 [伺服器]  、[建立新的]  ，然後指定下列資訊：

   - **伺服器名稱**：為 SQL 伺服器選擇唯一的名稱；此名稱將用來建立完整網域名稱，例如 wingtiptoyssql.database.windows.net  。
   - **伺服器管理員登入**：指定資料庫管理員的名稱。
   - **密碼**和**確認密碼**：指定資料庫管理員的密碼。
   - **位置**：指定與資料庫最為接近的地理區域。


1. 一旦您輸入上述所有資訊後，按一下 [確定]  。

1. 按一下 [檢閱及建立]  。

1. 檢閱設定，然後按一下 [建立]  。

### <a name="configure-a-firewall-rule-for-your-sql-server-using-the-azure-portal"></a>使用 Azure 入口網站設定 SQL 伺服器的防火牆規則

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]  ，然後按一下您剛才建立的 SQL 伺服器。

1. 在左側瀏覽窗格中，按一下 [概觀]  區段，然後按一下 [設定伺服器防火牆] 

   ![顯示防火牆設定][SQL06]

1. 在 [防火牆和虛擬網路]  區段中，藉由指定規則的唯一名稱來建立新的規則，然後輸入需要存取資料庫的 IP 位址範圍，再按一下 [儲存]  。 (在此練習中，IP 位址是您的開發電腦 (亦即用戶端) 的 IP 位址。  您可以將該 IP 位址用於**起始 IP 位址**和**結束 IP 位址**。)

   ![設定防火牆設定][SQL07]

### <a name="retrieve-the-connection-string-for-your-sql-server-using-the-azure-portal"></a>使用 Azure 入口網站擷取 SQL 伺服器的連接字串

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站並登入。

1. 按一下 [所有資源]  ，然後按一下您剛才建立的 SQL 資料庫。

1. 按一下 [連接字串]  ，然後按一下 [JDBC]  ，並複製 [JDBC] 文字欄位中的值。

   ![擷取 JDBC 連接字串][SQL09]

### <a name="create-test-table-in-database"></a>在資料庫中建立測試資料表
若要針對此資料庫執行用戶端應用程式，請使用下列 SQL 命令來建立新的資料表。

``` SQL
IF NOT EXISTS (SELECT 1 FROM sysobjects WHERE NAME='pet' and XTYPE='U')
  CREATE TABLE pet (
    id      INT           IDENTITY  PRIMARY KEY,
    name    VARCHAR(255),
    species VARCHAR(255)
  );

```

## <a name="configure-the-sample-application"></a>設定範例應用程式

1. 開啟命令殼層，並使用 git 命令複製範例專案，如下列範例所示：

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-jdbc-on-azure.git
   ```

1. 修改 POM 檔案以包含下列相依性：

```
 <dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.4.1.jre11</version>
 </dependency>
```
1. 在範例專案的 [資源]  目錄中尋找 application.properties  檔案，如果該檔案不存在，則加以建立。

1. 在文字編輯器中開啟 application.properties  檔案、在檔案中新增或設定下列幾行，然後使用本文稍早的適當值來取代範例值：

   ```yaml
   spring.datasource.driver-class-name=com.microsoft.sqlserver.jdbc.SQLServerDriver
   spring.datasource.url=jdbc:sqlserver://wingtiptoyssql.database.windows.net:1433;database=wingtiptoys;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
   spring.datasource.username=wingtiptoysuser@wingtiptoyssql
   spring.datasource.password=********
    ```
   其中：

   | 參數 | 描述 |
   |---|---|
   | `spring.datasource.url` | 指定本文稍早所編輯的 SQL JDBC 字串版本。 |
   | `spring.datasource.username` | 指定本文稍早所述的 SQL 管理員名稱，並對其附加縮略的伺服器名稱。 |
   | `spring.datasource.password` | 指定本文稍早所述的 SQL 管理員密碼。 |

1. 儲存並關閉 *application.properties* 檔案。

## <a name="package-and-test-the-sample-application"></a>封裝和測試應用程式範例 

1. 使用 Maven 建置應用程式範例；例如：

   ```shell
   mvn clean package -P sql
   ```

1. 啟動應用程式範例；例如：

   ```shell
   java -jar target/spring-data-jdbc-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 從命令提示字元使用 `curl` 建立新的記錄，如下列範例所示：

   ```shell
   curl -s -d '{"name":"dog","species":"canine"}' -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   或者：

``` shell
   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   應用程式應該會傳回如下所示的值：

   ```shell
   Added Pet(id=1, name=dog, species=canine).

   Added Pet(id=2, name=cat, species=feline).
   ```

1. 從命令提示字元使用 `curl` 擷取所有現有的記錄，如下列範例所示：

   ```shell
   curl -s http://localhost:8080/pets
   ```
    
   應用程式應該會傳回如下所示的值：

   ```json
   [{"id":1,"name":"dog","species":"canine"},{"id":2,"name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>摘要

在此教學課程中，您已建立使用 Spring Data 的範例 Java 應用程式，以在 Azure SQL 資料庫中使用 JDBC 儲存和擷取資訊。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>其他資源

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

<!-- URL List -->

[適用於 Java 開發人員的 Azure]: /azure/java/
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[SQL01]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-01.png
[SQL02]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-02.png
[SQL03]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-03.png
[SQL04]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-04.png
[SQL05]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-05.png
[SQL06]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-06.png
[SQL07]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-07.png
[SQL08]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-08.png
[SQL09]: media/configure-spring-data-jdbc-with-azure-sql-server/create-azure-sql-09.png
