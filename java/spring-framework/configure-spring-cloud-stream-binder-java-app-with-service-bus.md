---
title: 如何使用適用於 Azure 服務匯流排的 Spring Cloud Azure Stream Binder
description: 本文示範如何使用 Spring Cloud Stream Binder，將訊息傳送至 Azure 服務匯流排及從中接收訊息。
author: seanli1988
manager: kyliel
ms.author: Sean.Li
ms.date: 08/21/2019
ms.devlang: java
ms.service: azure-java
ms.topic: article
ms.openlocfilehash: 48bb5ee53c88910528ad2ed7f06c626e0a431275
ms.sourcegitcommit: f519a1ee8017850b2fa37049af3bac1ea5ca5516
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2019
ms.locfileid: "69892382"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>如何使用適用於 Azure 服務匯流排的 Spring Cloud Azure Stream Binder

[!INCLUDE [spring-boot-20-note.md](../includes/spring-boot-20-note.md)]

Azure 提供名為 [Azure 服務匯流排](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) (「服務匯流排」) 的非同步訊息平台，其以[先進的訊息佇列通訊協定 1.0](http://www.amqp.org/) ("AMQP 1.0") 標準為基礎。 服務匯流排可以跨支援的 Azure 平台範圍使用。

本文示範如何使用 Spring Cloud Stream Binder，將訊息傳送至 Azure 服務匯流排 `queues` 和 `topics` 及從中接收訊息。

## <a name="prerequisites"></a>必要條件

本文章需要下列必要條件：

1. 如果您還沒有 Azure 訂用帳戶，則可以啟用 [MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)或註冊[免費帳戶](https://azure.microsoft.comfree/)。

1. 支援的 JAVA 開發工具組 (JDK) 第 8 版或更新版本。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。

1. Apache 的 [Maven](http://maven.apache.org/) 3.2 版或更新版本。

1. 如果您已經設定服務匯流排的佇列或主題，請確定服務匯流排命名空間符合下列需求：

    1. 允許從所有網路存取
    1. 為進階 (或更高版本)
    1. 具有佇列和主題讀取/寫入存取權的存取原則

1. 如果您沒有已設定的服務匯流排佇列或主題，請使用 Azure 入口網站來[建立服務匯流排佇列](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quickstart-portal)或[建立服務匯流排主題](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)。 確定命名空間符合上一個步驟中所指定的需求。 此外，請記下命名空間中的連接字串，因為您在本教學課程的測試應用程式中需要用到它。

1. 如果您沒有 Spring Boot 應用程式，請[使用 Spring 初始設定式來建立 **Maven** 專案](https://start.spring.io/)。 請記得選取 [Maven專案]  ，然後在 [相依性]  下新增 **Web** 相依性。

## <a name="use-the-spring-cloud-stream-binder-starter"></a>使用 Spring Cloud Stream Binder Starter

1. 在應用程式的父系目錄中尋找 pom.xml  檔案；例如：

    `C:\SpringBoot\servicebus\pom.xml`

    -或-

    `/users/example/home/servicebus/pom.xml`

1. 在文字編輯器中開啟 pom.xml  檔案。

1. 根據您使用的是服務匯流排佇列或主題，將下列程式碼區塊新增至 **&lt;** 相依性> 元素底下：

    **服務匯流排佇列**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-queue-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![編輯服務匯流排佇列的 pom .xml 檔案。](./media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-queue.png)

    **服務匯流排主題**

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>spring-cloud-azure-servicebus-topic-stream-binder</artifactId>
        <version>1.1.0.RC5</version>
    </dependency>
    ```

    ![編輯服務匯流排主題的 pom .xml 檔案。](./media/configure-spring-cloud-stream-binder-java-app-with-service-bus/add-stream-binder-starter-pom-file-dependency-for-service-bus-topic.png)

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="configure-the-app-for-your-service-bus"></a>設定服務匯流排的應用程式

您可以根據連接字串或認證檔案來設定您的應用程式。 本教學課程使用連接字串。 如需使用認證檔案的詳細資訊，請參閱[適用於服務匯流排佇列的 Spring Cloud Azure Stream Binder 程式碼範例](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-queue-binder-sample#credential-file-based-usage
)和[適用於服務匯流排主題的 Spring Cloud Azure Stream Binder 程式碼範例](https://github.com/microsoft/spring-cloud-azure/tree/release/1.1.0.RC4/spring-cloud-azure-samples/servicebus-topic-binder-sample#credential-file-based-usage)。

1. 在應用程式的 [資源]  目錄中尋找 *application.properties* 檔案；例如：

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   -或-

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. 在文字編輯器中開啟 application.properties  檔案。

1. 根據您使用的是服務匯流排的佇列或主題，將適當的程式碼附加至 application.properties  檔案的結尾。 使用 [欄位描述](#fd) 資料表，以服務匯流排的適當屬性來取代範例值。

    **服務匯流排佇列**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=examplequeue
    spring.cloud.stream.bindings.output.destination=examplequeue
    spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **服務匯流排主題**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=exampletopic
    spring.cloud.stream.bindings.input.group=examplesubscription
    spring.cloud.stream.bindings.output.destination=exampletopic
    spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **<a name="fd">欄位描述</a>**

    |                                        欄位                                   |                                                                                   說明                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        指定您在服務匯流排命名空間中從 Azure 入口網站取得的連接字串。                                   |
    |               `spring.cloud.stream.bindings.input.destination`                 |                            指定您在本教學課程中使用的服務匯流排佇列或服務匯流排主題。                         |
    |                  `spring.cloud.stream.bindings.input.group`                    |                                            如果您使用服務匯流排主題，請指定主題訂用帳戶。                                |
    |               `spring.cloud.stream.bindings.output.destination`                |                               指定用於輸入目的地的相同值。                        |
    | `spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode` |                                                       指定 `MANUAL`。                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode` |                                                       指定 `MANUAL`。                                                   |

1. 儲存並關閉 *application.properties* 檔案。

## <a name="implement-basic-service-bus-functionality"></a>實作基本服務匯流排功能

在本節中，您會建立必要的 Java 類別，以便將訊息傳送至服務匯流排。

### <a name="modify-the-main-application-class"></a>修改主要應用程式類別

1. 在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：

    `C:\SpringBoot\servicebus\src\main\java\com\example\ServiceBusBinderApplication.java`

   -或-

   `/users/example/home/servicebus/src/main/java/com/example/ServiceBusBinderApplication.java`

1. 在文字編輯器中開啟主要應用程式 JAVA 檔案。

1. 將下列程式碼新增至該檔案：

    ```java
    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusBinderApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusBinderApplication.class, args);
        }
    }
    ```

1. 儲存並關閉檔案。

### <a name="create-a-new-class-for-the-source-connector"></a>建立新的來源連接器類別

1. 使用文字編輯器，在應用程式的 package 目錄中建立名為 StreamBinderSource.java  的 Java 檔案。

1. 將下列程式碼新增至新檔案：

    ```java
    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @EnableBinding(Source.class)
    @RestController
    public class StreamBinderSource {

        @Autowired
        private Source source;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```

1. 儲存並關閉 StreamBinderSources.java  檔案。

### <a name="create-a-new-class-for-the-sink-connector"></a>建立新的接收連接器類別

1. 使用文字編輯器，在應用程式的 package 目錄中建立名為 StreamBinderSink.java  的 Java 檔案。

1. 將下列程式碼行新增至新檔案：

    ```java
    package com.example;

    import com.microsoft.azure.spring.integration.core.AzureHeaders;
    import com.microsoft.azure.spring.integration.core.api.Checkpointer;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.annotation.StreamListener;
    import org.springframework.cloud.stream.messaging.Sink;
    import org.springframework.messaging.handler.annotation.Header;

    @EnableBinding(Sink.class)
    public class StreamBinderSink {

        @StreamListener(Sink.INPUT)
        public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
            System.out.println(String.format("New message received: '%s'", message));
            checkpointer.success().handle((r, ex) -> {
                if (ex == null) {
                    System.out.println(String.format("Message '%s' successfully checkpointed", message));
                }
                return null;
            });
        }
    }
    ```

1. 儲存並關閉 StreamBinderSink.java  檔案。

## <a name="build-and-test-your-application"></a>建置與測試您的應用程式

1. 開啟命令提示字元。

1. 將目錄切換到 pom.xml  的位置；例如：

    `cd C:\SpringBoot\servicebus`

    -或-

    `cd /users/example/home/servicebus`

2. 使用 Maven 建置 Spring Boot 應用程式並加以執行：

    ```shell
    mvn clean spring-boot:run
    ```

3. 當您的應用程式執行時，可以使用 cURL  來測試您的應用程式：

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    您應該會看到 "hello" 已發佈至應用程式記錄：

    ```shell
    New message received: 'hello'
    Message 'hello' successfully checkpointed
    ```

## <a name="clean-up-resources"></a>清除資源

當不再需要時，請使用 [Azure 入口網站](http://ms.portal.azure.com/)來刪除在本文中建立的資源，以避免產生非預期的費用。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)