---
title: 如何對 Azure 服務匯流排 JMS 使用 Spring Boot Starter
description: 本文示範如何使用 Spring JMS Starter，將訊息傳送至 Azure 服務匯流排及從中接收訊息。
author: seanli1988
manager: kyliel
ms.author: Sean.Li
ms.date: 08/21/2019
ms.devlang: java
ms.service: azure-java
ms.topic: article
ms.openlocfilehash: 58d8dd00deeb90b1a1b8935bcbbab471255328d4
ms.sourcegitcommit: 9cd460ee16b637e701aa30078932878c0d0a7945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2019
ms.locfileid: "70181983"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-service-bus-jms"></a>如何對 Azure 服務匯流排 JMS 使用 Spring Boot Starter

[!INCLUDE [spring-boot-20-note.md](../includes/spring-boot-20-note.md)]

Azure 提供名為 [Azure 服務匯流排](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-overview) (「服務匯流排」) 的非同步訊息平台，其以[先進的訊息佇列通訊協定 1.0](http://www.amqp.org/) ("AMQP 1.0") 標準為基礎。 服務匯流排可以跨支援的 Azure 平台範圍使用。

適用於 Azure 服務匯流排 JMS 的 Spring Boot Starter 提供與服務匯流排的 Spring 整合。

本文示範如何使用適用於 Azure 服務匯流排 JMS 的 Spring Boot Starter，在服務匯流排 `queues` 和 `topics` 之間傳送和接收訊息。

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

## <a name="use-the-azure-service-bus-jms-starter"></a>使用 Azure 服務匯流排 JMS Starter

1. 在應用程式的父系目錄中尋找 pom.xml  檔案；例如：

    `C:\SpringBoot\servicebus\pom.xml`

    -或-

    `/users/example/home/servicebus/pom.xml`

1. 在文字編輯器中開啟 pom.xml  檔案。

1. 將 Spring Boot Azure 服務匯流排 JMS Starter 新增至 `<dependencies>` 的清單：

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms-spring-boot-starter</artifactId>
        <version>2.1.7</version>
    </dependency>
    ```

    ![將相依性區段新增至 pom.xml 檔案。](./media/configure-spring-boot-starter-java-app-with-azure-service-bus/add-dependency-section-new.png)

1. 儲存並關閉 *pom.xml* 檔案。

## <a name="configure-the-app-for-your-service-bus"></a>設定服務匯流排的應用程式 

在本節中，您會了解如何設定您的應用程式，以使用服務匯流排的佇列或主題。

### <a name="use-a-service-bus-queue"></a>使用服務匯流排佇列

1. 在應用程式的 resources  目錄中尋找 *application.properties*；例如：

    `C:\SpringBoot\servicebus\application.properties`

    -或-

    `/users/example/home/servicebus/application.properties`

1. 在文字編輯器中開啟 application.properties  檔案。

1. 將下列程式碼附加至 application.properties  檔案的結尾。 將範例值取代為您的服務匯流排的適當值：

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **欄位描述**

    | 欄位                                     | 說明                                                                                     |
    |-------------------------------------------|-------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | 指定您在服務匯流排命名空間中從 Azure 入口網站取得的連接字串。 |
    | `spring.jms.servicebus.idle-timeout`      | 指定閒置逾時 (以毫秒為單位)。 本教學課程的建議值為 1800000。   |

1. 儲存並關閉 *application.properties* 檔案。

### <a name="use-service-bus-topic"></a>使用服務匯流排主題

1. 在應用程式的 resources  目錄中尋找 *application.properties*；例如：

    `C:\SpringBoot\servicebus\application.properties`

    -或-

    `/users/example/home/servicebus/application.properties`

1. 在文字編輯器中開啟 application.properties  檔案。

1. 將下列程式碼附加至 application.properties  檔案的結尾。 將範例值取代為您的服務匯流排的適當值：

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.topic-client-id=<ServiceBusTopicClientId>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **欄位描述**

    | 欄位                                     | 說明                                                                                       |
    |-------------------------------------------|---------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | 指定您在服務匯流排命名空間中從 Azure 入口網站取得的連接字串。   |
    | `spring.jms.servicebus.topic-client-id`   | 如果您使用的是具有長期訂用帳戶的 Azure 服務匯流排主題，請指定 JMS 用戶端識別碼。 |
    | `spring.jms.servicebus.idle-timeout`      | 指定閒置逾時 (以毫秒為單位)。 本教學課程的建議值為 1800000。     |

1. 儲存並關閉 *application.properties* 檔案。

## <a name="implement-basic-service-bus-functionality"></a>實作基本服務匯流排功能

在本節中，您會建立必要的 JAVA 類別，以便將訊息傳送至服務匯流排佇列或主題，並從對應的佇列或主題訂用帳戶接收訊息。

### <a name="modify-the-main-application-class"></a>修改主要應用程式類別

1. 在應用程式的封裝目錄中尋找主要應用程式 Java 檔案；例如：

    `C:\SpringBoot\servicebus\src\main\java\com\wingtiptoys\servicebus\ServiceBusJmsStarterApplication.java`

    -或-

    `/users/example/home/servicebus/src/main/java/com/wingtiptoys/servicebus/ServiceBusJmsStarterApplication.java`

1. 在文字編輯器中開啟主要應用程式 JAVA 檔案。

1. 將下列程式碼新增至該檔案：

   ```java
    package com.wingtiptoys.servicebus;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusJmsStarterApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusJmsStarterApplication.class, args);
        }
    }
    ```

1. 儲存並關閉檔案。

### <a name="define-a-test-java-class"></a>定義測試 JAVA 類別

1. 使用文字編輯器，在應用程式的 package 目錄中建立名為 User.java  的 Java 檔案。

1. 定義用來儲存和擷取使用者名稱的一般使用者類別：

    ```java
    package com.wingtiptoys.servicebus;

    import java.io.Serializable;

    // Define a generic User class.
    public class User implements Serializable {

        private static final long serialVersionUID = -295422703255886286L;

        private String name;

        public User() {
        }

        public User(String name) {
            setName(name);
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

    }
    ```

    系統會實作 `Serializable`，以在 Spring 架構中的 `JmsTemplate` 使用 `send` 方法。 否則，應該定義自訂的 `MessageConverter` Bean，以文字格式將內容序列化為 json。 如需 `MessageConverter` 的詳細資訊，請參閱官方 [Spring JMS Starter 專案](https://spring.io/guides/gs/messaging-jms/)。

1. 儲存並關閉 *User.java* 檔案。

### <a name="create-a-new-class-for-the-message-send-controller"></a>建立訊息傳送控制器的新類別

1. 使用文字編輯器，在應用程式的 package 目錄中建立名為 SendController.java  的 Java 檔案

1. 將下列程式碼新增至新檔案：

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jms.core.JmsTemplate;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class SendController {

        private static final String DESTINATION_NAME = "<DestinationName>";

        private static final Logger logger = LoggerFactory.getLogger(SendController.class);

        @Autowired
        private JmsTemplate jmsTemplate;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            logger.info("Sending message");
            jmsTemplate.convertAndSend(DESTINATION_NAME, new User(message));
            return message;
        }
    }
    ```

    > [!NOTE]
    > 將 `<DestinationName>` 取代為您自己在服務匯流排命名空間中設定的佇列名稱或主題名稱。

1. 儲存並關閉 SendController  。

### <a name="create-a-class-for-the-message-receive-controller"></a>建立訊息接收控制器的類別

#### <a name="receive-messages-from-a-service-bus-queue"></a>從服務匯流排佇列接收訊息

1. 使用文字編輯器，在應用程式的 package 目錄中建立名為 QueueReceiveController. java  的 Java 檔案

1. 將下列程式碼新增至新檔案：

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class QueueReceiveController {

        private static final String QUEUE_NAME = "<ServiceBusQueueName>";

        private final Logger logger = LoggerFactory.getLogger(QueueReceiveController.class);

        @JmsListener(destination = QUEUE_NAME, containerFactory = "jmsListenerContainerFactory")
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

    > [!NOTE]
    > 將 `<ServiceBusQueueName>` 取代為您自己在服務匯流排命名空間中設定的佇列名稱。

1. 儲存並關閉 QueueReceiveController.java  檔案。

#### <a name="receive-messages-from-a-service-bus-subscription"></a>從服務匯流排訂用帳戶接收訊息

1. 使用文字編輯器，在應用程式的 package 目錄中建立名為 TopicReceiveController.java  的 Java 檔案。 

1. 將下列程式碼新增至新檔案。 將 `<ServiceBusTopicName>` 預留位置取代為您自己在服務匯流排命名空間中設定的主題名稱。 將 `<ServiceBusSubscriptionName>` 預留位置取代為您自己的服務匯流排主題訂用帳戶名稱。

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class TopicReceiveController {

        private static final String TOPIC_NAME = "<ServiceBusTopicName>";

        private static final String SUBSCRIPTION_NAME = "<ServiceBusSubscriptionName>";

        private final Logger logger = LoggerFactory.getLogger(TopicReceiveController.class);

        @JmsListener(destination = TOPIC_NAME, containerFactory = "topicJmsListenerContainerFactory",
                subscription = SUBSCRIPTION_NAME)
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

1. 儲存並關閉 TopicReceiveController.java  檔案。

## <a name="build-and-test-your-application"></a>建置與測試您的應用程式

1. 開啟命令提示字元，並將目錄切換到 pom.xml  的位置；例如：

    `cd C:\SpringBoot\servicebus`

    -或-

    `cd cd /users/example/home/servicebus`

1. 使用 Maven 建置 Spring Boot 應用程式並加以執行：

    ```shell
    mvn clean spring-boot:run
    ```

1. 當您的應用程式執行時，可以使用 cURL  來測試您的應用程式：

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    您應該會看到「正在傳送訊息」和「hello」張貼到您的應用程式記錄：

    ```shell
    [nio-8080-exec-1] com.wingtiptoys.servicebus.SendController : Sending message
    [enerContainer-1] com.wingtiptoys.servicebus.ReceiveController : Received message: hello
    ```

## <a name="clean-up-resources"></a>清除資源

當不再需要時，請使用 [Azure 入口網站](http://ms.portal.azure.com/)來刪除在本文中建立的資源，以避免產生非預期的費用。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [如何搭配服務匯流排和 AMQP 1.0 使用 JMS API](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)