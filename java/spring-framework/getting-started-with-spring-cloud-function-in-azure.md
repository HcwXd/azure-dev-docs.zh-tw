---
title: 在 Azure 中開始使用 Spring Cloud Function
description: 了解如何在 Azure 中使用 Spring Cloud Function。
services: ''
documentationcenter: java
author: judubois
manager: brborges
ms.assetid: ''
ms.author: judubois
ms.date: 07/17/2019
ms.devlang: Java
ms.service: azure-functions
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: na
ms.openlocfilehash: fc62ce6d9416e2031fbefef92a4613fb0ff9cc43
ms.sourcegitcommit: 4cc7f5e1e4601065bfcb4c2eeb7d47ad0bec61f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68433158"
---
# <a name="getting-started-with-spring-cloud-function-in-azure"></a>在 Azure 中開始使用 Spring Cloud Function

本文會引導您使用 [Spring Cloud Function](https://spring.io/projects/spring-cloud-function) 來開發 Java 函式並將其發佈至 Azure Functions。 當您完成時，您的函式程式碼會在 Azure 中的[取用方案](/azure/azure-functions/functions-scale#consumption-plan)上執行，並且可使用 HTTP 要求來觸發。

[!INCLUDE [quickstarts-free-trial-note](../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>必要條件

若要使用 Java 開發函式，您必須安裝下列項目：

- [Java Developer Kit](https://aka.ms/azure-jdks) 第 8 版
- [Apache Maven](https://maven.apache.org) 3.0 版或更新版本
- [Azure CLI](https://docs.microsoft.com/cli/azure)
- [Azure Functions Core Tools](/azure/azure-functions/functions-run-local#v2) 2.7.1158 版或更新版本

> [!IMPORTANT]
> JAVA_HOME 環境變數必須設定為 JDK 的安裝位置，才能完成本快速入門。

## <a name="what-we-are-going-to-build"></a>我們即將建立的項目

我們即將建立一個傳統 "Hello, World" 函式，它會在 Azure Functions 上執行，並以 Spring Cloud Function 進行設定。

它會接收一個包含使用者名稱的簡單 `User`JSON 物件，並傳回 `Greeting` 物件(其中包含給該使用者的歡迎訊息)。

我們在此建立的專案可在 [https://github.com/Azure-Samples/hello-spring-function-azure](https://github.com/Azure-Samples/hello-spring-function-azure)取得，所以如果您想要查看本快速入門中詳述的最終工作，可以直接使用該範例存放庫。

## <a name="create-a-new-maven-project"></a>建立新的 Maven 專案

我們即將建立空的 Maven 專案，並使用 Spring Cloud Function 和 Azure Functions 進行設定。

在空的資料夾中，建立新的 *pom.xml*，並複製/貼上範例專案中的內容 ([https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml](https://github.com/Azure-Samples/hello-spring-function-azure/blob/master/pom.xml))。

> [!NOTE]
> 此檔案會使用來自 Spring Boot 和 Spring Cloud Function 的 Maven 相依性，並設定 Spring Boot 和 Azure Functions Maven 外掛程式。

您必須針對您的應用程式自訂幾個屬性：

- `<functionAppName>` 是您的 Azure 函式名稱
- `<functionAppRegion>` 是您的函式部署所在的 Azure 區域名稱
- `<functionResourceGroup>` 是您要使用的 Azure 資源群組名稱

您應該直接在 *pom.xml* 檔案的頂端附近變更這些屬性：

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <azure.functions.maven.plugin.version>1.3.2</azure.functions.maven.plugin.version>
    <azure.functions.java.library.version>1.3.0</azure.functions.java.library.version>
    <functionAppName>my-spring-function</functionAppName>
    <functionAppRegion>westus</functionAppRegion>
    <stagingDirectory>${project.build.directory}/azure-functions/${functionAppName}</stagingDirectory>
    <functionResourceGroup>my-resource-group</functionResourceGroup>
    <start-class>com.example.HelloFunction</start-class>
    <wrapper.version>1.0.22.RELEASE</wrapper.version>
</properties>
```

## <a name="create-azure-configuration-files"></a>建立 Azure 組態檔

建立 src/main/azure  資料夾，並在其中新增下列 Azure Functions 組態檔。

*host.json*：

```json
{
  "version": "2.0",
  "functionTimeout": "00:10:00"
}
```

*local.settings.json*：

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "FUNCTIONS_WORKER_RUNTIME": "java",
    "AzureWebJobsDashboard": ""
  }
}
```

## <a name="create-domain-objects"></a>建立網域物件

Azure Functions 可以接收和傳送 JSON 格式的物件。
我們現在要建立 `User` 和 `Greeting` 物件，其代表我們的網域模型。
如果您想自訂本快速入門並讓它變得更有趣，您可建立更複雜的物件 (包含更多屬性)。

建立 src/main/java/com/example/model  資料夾並新增下列兩個檔案：

*User.java*：

```java
package com.example.model;

public class User {

    public User() {
    }

    public User(String name) {
        this.name = name;
    }

    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

*Greeting.java*：

```java
package com.example.model;

public class Greeting {

    public Greeting() {
    }

    public Greeting(String message) {
        this.message = message;
    }

    private String message;

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
```

## <a name="create-the-spring-boot-application"></a>建立 Spring Boot 應用程式

此應用程式將會管理所有商務邏輯，並可存取完整的 Spring Boot 生態系統。
因此，您就能透過標準 Azure 函式取得兩個主要優點：

- 它不會依賴 Azure Functions API，因此可以輕鬆地移植到其他系統。 例如，它可以在正常的 Spring Boot 應用程式中重複使用。
- 它可以使用來自 Spring Boot 的所有 `@Enable` 註釋，輕鬆地新增強大的新功能。

在 src/main/java/com/example  資料夾中，建立下列檔案，這是正常的 Spring Boot 應用程式：

*HelloFunction.java*：

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.util.function.Function;

@SpringBootApplication
public class HelloFunction {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(HelloFunction.class, args);
    }

    @Bean
    public Function<User, Greeting> hello() {
        return user -> new Greeting("Welcome, " + user.getName());
    }
}
```

> [!NOTE] 
> `hello()` 函式相當明確：
> 
> - 它會傳回 `java.util.function.Function`，這是將在本快速入門中使用的函式。 它包含商務邏輯，且使用標準 Java API 將一個物件轉換成另一個物件。
> - 當它具有 `@Bean` 註釋時，它就是一個 Spring Bean，且其名稱預設為其中一個方法 (`hello`)。 如果您想要在應用程式中建立其他函式，這就很重要，因為此名稱必須符合我們將在下一節中建立的 Azure Functions 名稱。

## <a name="create-the-azure-function"></a>建立 Azure 函式

為了受惠於完整的 Azure Functions API，我們會立即將特定類別編碼：這是一個 Azure 函式，它會將其執行作業委派給我們在上一個步驟中建立的 Spring Cloud Function。

在 src/main/java/com/example  資料夾中，建立下列 Azure 函式：

*HelloHandler.java*：

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import com.microsoft.azure.functions.ExecutionContext;
import com.microsoft.azure.functions.HttpMethod;
import com.microsoft.azure.functions.HttpRequestMessage;
import com.microsoft.azure.functions.annotation.AuthorizationLevel;
import com.microsoft.azure.functions.annotation.FunctionName;
import com.microsoft.azure.functions.annotation.HttpTrigger;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import java.util.Optional;

public class HelloHandler extends AzureSpringBootRequestHandler<User, Greeting> {

    @FunctionName("hello")
    public Greeting execute(
            @HttpTrigger(name = "request", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<User>> request,
            ExecutionContext context) {

        context.getLogger().info("Greeting user name: " + request.getBody().get().getName());
        return handleRequest(request.getBody().get(), context);
    }
}
```

此 Java 類別是 Azure 函式，具有下列有趣功能：

- 它會擴充 `AzureSpringBootRequestHandler`，其可連結 Azure Functions 與 Spring Cloud Function。 這會提供其 `execute()` 方法中使用的 `handleRequest()` 方法。
- 函式的名稱 (如 `@FunctionName("hello")` 註釋所定義) 與上一個步驟中所設定的 Spring Bean (`hello`) 相同。
- 這是真實的 Azure 函式，因此您可以在此使用完整的 Azure Functions API。

## <a name="add-unit-tests"></a>新增單元測試

當然，這是選擇性步驟，但身為良好的開發人員，您應該新增單元測試來驗證應用程式是否正常運作。

建立 src/test/java/com/example  資料夾並新增下列 JUnit 測試：

*HelloFunctionTest. java*：

```java
package com.example;

import com.example.model.Greeting;
import com.example.model.User;
import org.junit.Test;
import org.springframework.cloud.function.adapter.azure.AzureSpringBootRequestHandler;

import static org.assertj.core.api.Assertions.assertThat;

public class HelloFunctionTest {

    @Test
    public void test() {
        Greeting result = new HelloFunction().hello().apply(new User("foo"));
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }

    @Test
    public void start() throws Exception {
        AzureSpringBootRequestHandler<User, Greeting> handler = new AzureSpringBootRequestHandler<>(
                HelloFunction.class);
        Greeting result = handler.handleRequest(new User("foo"), null);
        handler.close();
        assertThat(result.getMessage()).isEqualTo("Welcome, foo");
    }
}
```

您現在可以使用 Maven 測試您的 Azure 函式：

```bash
mvn clean test
```

## <a name="run-the-function-locally"></a>在本機執行函式

將應用程式部署至 Azure 函式之前，讓我們先在本機進行測試。

首先，您必須將應用程式封裝成 Jar 檔案：

```bash
mvn package
```

現在已封裝應用程式，您可以使用 `azure-functions` Maven 外掛程式來執行它：

```bash
mvn azure-functions:run
```

此 Azure 函式現在應該適用於您的 localhost (使用連接埠 7071)。 您可以使用 JSON 格式的 `User` 物件，將 POST 要求傳送給該函式，藉此進行測試。 例如，使用 cURL：

```bash
curl http://localhost:7071/api/hello -d "{\"name\":\"Azure\"}"
```

函式應該會以 `Greeting` 物件回答您，但仍採用 JSON 格式：

```Output
{
  "message": "Welcome, Azure"
}
```

以下是畫面頂端的 cURL 要求螢幕擷取畫面，以及底部的本機 Azure 函式：

 ![在本機執行的 Azure 函式][RFL01]

## <a name="deploy-the-function-to-azure-functions"></a>將函式部署到 Azure Functions

您現在要將 Azure 函式發佈到實際執行環境。 請記住，您在 *pom.xml* 中定義的 `<functionAppName>`、`<functionAppRegion>` 和 `<functionResourceGroup>` 屬性將用來設定您的函式。

執行 Maven 以自動部署函式：

```bash
mvn azure-functions:deploy
```

現在移至 [Azure 入口網站](https://portal.azure.com)，尋找已建立的 `Function App`。

按一下函式：

- 在函式概觀中，請注意函式的 URL。
- 選取 [平台功能]  索引標籤以尋找 [記錄串流]  服務，然後選取此服務來檢查執行中的函式。

現在，如同您在上一節中所做的一樣，使用 cURL 來存取執行中的函式。 請以您的真實函式名稱取代 `your-function-name`：

```bash
curl https:/your-function-name.azurewebsites.net/api/hello -d "{\"name\":\"Azure\"}"
```

如同上一節，函式應該會以 `Greeting` 物件回答您，但仍採用 JSON 格式：

```Output
{
  "message": "Welcome, Azure"
}
```

恭喜，您有一個在 Azure Functions 上執行的 Spring Cloud Function！

<!-- IMG List -->

[RFL01]: ./media/getting-started-with-spring-cloud-function-in-azure/RFL01.png
