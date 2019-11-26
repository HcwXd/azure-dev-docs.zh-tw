---
title: 如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter
description: 了解如何設定搭配 Azure Cosmos DB SQL API 使用 Spring Boot Initializer 建立的應用程式。
services: cosmos-db
documentationcenter: java
author: KarlErickson
manager: barbkess
ms.author: karler
ms.date: 10/02/2019
ms.devlang: java
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.openlocfilehash: 192550b74a35eb24620c58e86e6a55e86e5e90ab
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118204"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>如何搭配 Azure Cosmos DB SQL API 使用 Spring Boot Starter

Azure Cosmos DB 是一個橫跨全球的分散式資料庫服務，讓開發人員能夠利用各種不同的標準 API (例如 SQL、MongoDB、圖形和資料表 API) 來使用資料。 Microsoft 的 Spring Boot Starter 讓開發人員能夠使用 Spring Boot 應用程式，藉由使用 SQL API 來輕鬆地與 Azure Cosmos DB 整合。

本文將示範如何使用 Azure 入口網站建立 Azure Cosmos DB，接著使用 **[Spring Initializr]** 建立自訂的 Spring Boot 應用程式，然後將 [適用於 Azure 的 Spring Boot Cosmos DB Starter] 新增到您的自訂應用程式，以使用 Spring Data 和 Cosmos DB SQL API 將資料儲存在您的 Azure Cosmos DB 並從中擷取資料。

## <a name="prerequisites"></a>必要條件

若要遵循本文中的步驟，需要具備下列必要條件：

* Azure 訂用帳戶；如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益]或註冊[免費的 Azure 帳戶]。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>使用 Azure 入口網站建立 Azure Cosmos DB

1. 從 <https://portal.azure.com/> 瀏覽至 Azure 入口網站，然後按一下 [建立資源]  。

1. 依序按一下 [資料庫]  和 [Azure Cosmos DB]  。

    ![Azure 入口網站][AZ02]

1. 在 [Azure Cosmos DB]  頁面上，輸入下列資訊：

    * 選取您想要用於資料庫的**訂用帳戶**。
    * 指定是否要為資料庫建立新的**資源群組**，或選擇現有的資源群組。
    * 輸入唯一的**帳戶名稱**，您將使用此名稱作為資料庫的 URI。 例如：*wingtiptoysdata*。
    * 為 API 選擇**核心 (SQL)** 。
    * 指定資料庫的**位置**。

    在指定這些選項之後，按一下 [檢閱+建立]  ，檢閱您的規格，然後按一下 [建立]  。

    ![Azure 入口網站][AZ03]

1. 當您的資料庫建立之後，它會列在您的 Azure [儀表板]  中，以及 [所有資源]  和 [Azure Cosmos DB]  頁面下方。 您可以按一下這些任何位置上的資料庫，來開啟快取的屬性頁面。

1. 當資料庫的屬性頁面顯示時，按一下 [金鑰]  ，並複製資料庫的 URI 和存取金鑰；您將會在 Spring Boot 應用程式中使用這些值。

    ![Azure 入口網站][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 建立簡單的 Spring Boot 應用程式

請使用下列步驟，以 Azure 支援建立新的 Spring Boot 應用程式專案。 或者，您也可以使用 [azure-spring-boot](https://github.com/microsoft/azure-spring-boot) 存放庫中的 [azure-cosmosdb-spring-boot-sample](https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-samples/azure-cosmosdb-spring-boot-sample) 範例。 然後，您可以直接跳到[建置及測試您的應用程式](#build-and-test-your-app)。

1. 瀏覽至 <https://start.spring.io/>。

1. 指定您想要使用 **Java** 產生 **Maven 專案**指定您的 **Spring Boot** 版本、輸入應用程式的**群組**和**成品**名稱、在相依性中新增 **Azure 支援**，然後按一下按鈕以**產生專案**。

    ![Spring Initializr 的基本選項][SI01]

    > [!NOTE]
    >
    > Spring Initializr 會使用**群組**和**成品**名稱來建立套件名稱；例如：*com.example.wintiptoys*。

1. 出現提示時，將專案下載至本機電腦上的路徑，並將檔案解壓縮。

您的 Spring Boot 簡單應用程式現在已可供編輯。

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>設定 Spring Boot 應用程式以使用 Azure Spring Boot Starter

1. 在應用程式的目錄中尋找 *pom.xml* 檔案；例如：

    `C:\SpringBoot\wingtiptoysdata\pom.xml`

    -或-

    `/users/example/home/wingtiptoysdata/pom.xml`

1. 在文字編輯器中開啟 pom.xml  檔案，並將下列內容新增至 `<dependencies>` 元素：

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
    </dependency>

    <dependency>
        <groupId>io.projectreactor.netty</groupId>
        <artifactId>reactor-netty</artifactId>
        <version>0.8.3.RELEASE</version>
    </dependency>
    ```

1. 確認 properties  元素有指出所需的 Java 和 Azure 版本：

    ```xml
    <properties>
       <java.version>1.8</java.version>
       <azure.version>2.2.0.M1</azure.version>
    </properties>
    ```

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>設定 Spring Boot 應用程式以使用您的 Azure Cosmos DB

1. 在應用程式的 [資源]  目錄中尋找 *application.properties* 檔案；例如：

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    -或-

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. 在文字編輯器中開啟 *application.properties* 檔案、將下列數行新增至檔案中，然後使用您資料庫的適當屬性來取代範例值：

    ```text
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

    # Specify the access key for your database.
    azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

    # Specify the name of your database.
    azure.cosmosdb.database=wingtiptoysdata
    ```

1. 儲存並關閉 *application.properties* 檔案。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>新增範例程式碼來實作基本的資料庫功能

在本節中，您會建立兩個 Java 類別以供儲存使用者資料，然後修改您的主要應用程式類別，以建立*使用者*類別的執行個體，並將其儲存至您的資料庫。

### <a name="define-a-base-class-for-storing-user-data"></a>定義用來儲存使用者資料的基底類別

1. 在與主要應用程式 Java 檔案相同的目錄中，建立名為 *User.java* 的新檔案。

1. 在文字編輯器中開啟 *User.java* 檔案，然後下列數行新增至檔案中，以定義泛型使用者類別來儲存和擷取您資料庫中的值：

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document;
    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;

    @Document(collection = "mycollection")
    public class User {

        @Id
        private String id;
        private String firstName;

        @PartitionKey
        private String lastName;
        private String address;

        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
        }

        public User() {
        }

        public String getId() {
            return id;
        }

        public void setId(String id) {
            this.id = id;
        }

        public String getFirstName() {
            return firstName;
        }

        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }

        public String getLastName() {
            return lastName;
        }

        public void setLastName(String lastName) {
            this.lastName = lastName;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        @Override
        public String toString() {
            return String.format("%s %s, %s", firstName, lastName, address);
        }
    }
    ```

1. 儲存並關閉 *User.java* 檔案。

### <a name="define-a-data-repository-interface"></a>定義資料存放庫介面

1. 在與主要應用程式 Java 檔案相同的目錄中，建立名為 *UserRepository.java* 的新檔案。

1. 在文字編輯器中開啟 *UserRepository.java* 檔案，然後將下列數行新增至檔案中，以定義使用者存放庫介面來延伸預設的 `ReactiveCosmosRepository` 介面：

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.repository.ReactiveCosmosRepository;
    import org.springframework.stereotype.Repository;
    import reactor.core.publisher.Flux;

    @Repository
    public interface UserRepository extends ReactiveCosmosRepository<User, String> {
        Flux<User> findByFirstName(String firstName);
    }
    ```

    `ReactiveCosmosRepository` 介面會取代舊版 Starter 的 `DocumentDbRepository` 介面。 新介面會提供同步的回應式 API，以供您進行基本的儲存、刪除和尋找作業。

1. 儲存並關閉 *UserRepository.java* 檔案。

### <a name="modify-the-main-application-class"></a>修改主要應用程式類別

1. 在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：

    `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

    -或-

    `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

1. 在文字編輯器中開啟主要應用程式 Java 檔案，並將下列數行新增至檔案中：

    ```java
    package com.example.wingtiptoysdata;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;

    import javax.annotation.PostConstruct;
    import javax.annotation.PreDestroy;
    import java.util.Optional;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);

        @Autowired
        private UserRepository repository;

        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }

        public void run(String... var1) throws Exception {
            final User testUser = new User("1", "Tasha", "Calderon", "4567 Main St Buffalo, NY 98052");

            LOGGER.info("Saving user: {}", testUser);

            // Save the User class to Azure CosmosDB database.
            final Mono<User> saveUserMono = repository.save(testUser);

            final Flux<User> firstNameUserFlux = repository.findByFirstName("testFirstName");

            //  Nothing happens until we subscribe to these Monos.
            //  findById will not return the user as user is not present.
            final Mono<User> findByIdMono = repository.findById(testUser.getId());
            final User findByIdUser = findByIdMono.block();
            Assert.isNull(findByIdUser, "User must be null");

            final User savedUser = saveUserMono.block();
            Assert.state(savedUser != null, "Saved user must not be null");
            Assert.state(savedUser.getFirstName().equals(testUser.getFirstName()), "Saved user first name doesn't match");

            LOGGER.info("Saved user");

            firstNameUserFlux.collectList().block();

            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");

            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");

            LOGGER.info("Found user by findById : {}", result);
        }

        @PostConstruct
        public void setup() {
            LOGGER.info("Clear the database");
            this.repository.deleteAll().block();
        }

        @PreDestroy
        public void cleanup() {
            LOGGER.info("Cleaning up users");
            this.repository.deleteAll().block();
        }
    }
    ```

1. 儲存並關閉主要應用程式 Java 檔案。

## <a name="build-and-test-your-app"></a>建置及測試您的應用程式

1. 開啟命令提示字元，並瀏覽至 pom.xml  檔案所在的資料夾；例如：

    `cd C:\SpringBoot\wingtiptoysdata`

    -或-

    `cd /users/example/home/wingtiptoysdata`

1. 使用下列命令來建置並執行應用程式：

    ```console
    mvnw clean test
    ```

    此命令會在測試階段期間自動執行應用程式。 您也可以使用：

    ```console
    mvnw clean spring-boot:run
    ```

    在某些組建和測試輸出之後，主控台視窗便會顯示類似下列內容的訊息：

    ```console
      .   ____          _            __ _ _
     /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
     \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
     =========|_|==============|___/=/_/_/_/
     :: Spring Boot ::            (v2.2.0.RC1)
    >
    > 2019-10-04 15:19:06.817  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Starting WingtiptoysdataApplicationTests on devmachine03 with PID 30013 (started by <user> in /d/source/repos/wingtiptoysdata)
    > 2019-10-04 15:19:06.818  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : No active profile set, falling back to default profiles: default
    > 2019-10-04 15:19:08.329  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.720  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 1369ms. Found 1 repository interfaces.
    > 2019-10-04 15:19:09.734  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.748  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 13ms. Found 0 repository interfaces.

    ... (omitting Cosmos DB connection output) ...

    > 2019-10-04 15:19:46.584  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Started WingtiptoysdataApplicationTests in 40.702 seconds (JVM running for 44.647)
    > 2019-10-04 15:19:46.587  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saving user: Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > 2019-10-04 15:19:47.122  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saved user
    > 2019-10-04 15:19:47.289  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Found user by findById : Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 44.003 s - in com.example.wingtiptoysdata.WingtiptoysdataApplicationTests
    > 2019-10-04 15:19:48.124  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.194  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.200  INFO 30013 --- [extShutdownHook] c.e.w.WingtiptoysdataApplication         : Cleaning up users
    > [INFO]
    > [INFO] Results:
    > [INFO]
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    > [INFO]
    > [INFO]
    > [INFO] --- maven-jar-plugin:3.1.2:jar (default-jar) @ wingtiptoysdata ---
    > [INFO] Building jar: /d/source/repos/wingtiptoysdata/target/wingtiptoysdata-0.0.1-SNAPSHOT.jar
    > [INFO]
    > [INFO] --- spring-boot-maven-plugin:2.2.0.RC1:repackage (repackage) @ wingtiptoysdata ---
    > [INFO] Replacing main artifact with repackaged archive
    > [INFO] ------------------------------------------------------------------------
    > [INFO] BUILD SUCCESS
    > [INFO] ------------------------------------------------------------------------
    > [INFO] Total time:  02:18 min
    > [INFO] Finished at: 2019-10-04T15:20:05-07:00
    > [INFO] ------------------------------------------------------------------------
    ```

    ![已成功從應用程式輸出][JV02]

    `Saved user` 和 `Found user` 訊息指出系統已成功將資料儲存至 Cosmos DB，然後再次加以擷取。

## <a name="clean-up-resources"></a>清除資源

如果您不打算繼續使用此應用程式，請務必刪除稍早所建立 Cosmos DB 所在的資源群組。 您可以從 Azure 入口網站執行此動作。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>其他資源

如需使用 Azure Cosmos DB 和 Java 的詳細資訊，請參閱下列文章：

* [Azure Cosmos DB 文件]。

* [Azure Cosmos DB：使用 Java 和 Azure 入口網站建立文件資料庫][Build a SQL API app with Java]

* [適用於 Azure Cosmos DB SQL API 的 Spring 資料]

如需在 Azure 上使用 Spring Boot 應用程式的詳細資訊，請參閱下列文章：

* [適用於 Azure 的 Spring Boot Cosmos DB Starter]

* [將 Spring Boot 應用程式部署到 Azure App Service](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [在 Azure Container Service 的 Kubernetes 叢集上執行 Spring Boot 應用程式](deploy-spring-boot-java-app-on-kubernetes.md)

如需如何搭配使用 Azure 和 Java 的詳細資訊，請參閱[適用於 Java 開發人員的 Azure] 和[使用 Azure DevOps 和 Java]。

**[Spring Framework]** 是一個開放原始碼解決方案，可協助 Java 開發人員建立企業級應用程式。 [Spring Boot] 是建立在該平台基礎上更為熱門的專案之一，其中會提供用來建立獨立 Java 應用程式的簡化方法。 為了協助開發人員開始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了數個範例 Spring Boot 套件。 除了從基本的 Spring Boot 專案清單中進行選擇， **[Spring Initializr]** 還能協助開發人員開始建立自訂的 Spring Boot 應用程式。

<!-- URL List -->

[Azure Cosmos DB 文件]: /azure/cosmos-db/
[適用於 Java 開發人員的 Azure]: /azure/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[適用於 Azure Cosmos DB SQL API 的 Spring 資料]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[適用於 Azure 的 Spring Boot Cosmos DB Starter]: https://github.com/microsoft/azure-spring-boot/tree/master/azure-spring-boot-starters/azure-cosmosdb-spring-boot-starter
[免費的 Azure 帳戶]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[MSDN 訂戶權益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png

[JV02]: ./media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
