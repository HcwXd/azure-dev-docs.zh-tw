---
title: 開始對 Azure 上執行的 Java 應用程式使用 Logz.io
description: 本教學課程將示範如何為 Azure 上執行的 Java 應用程式整合及設定 Logz.io。
author: jdubois
manager: bborges
ms.devlang: java
ms.topic: tutorial
ms.service: azure
ms.date: 11/05/2019
ms.author: judubois
ms.openlocfilehash: 263a328866d36fd60e2ab7cc9fbe8fa8af45b9d4
ms.sourcegitcommit: 794f7f72947034944dc4a5d19baa57d905a16ab0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73957201"
---
# <a name="tutorial-getting-started-with-monitoring-and-logging-using-logzio-for-java-apps-running-on-azure"></a>教學課程：針對在 Azure 上執行的 Java 應用程式，使用 Logz.io 開始進行監視和記錄

本教學課程將說明如何設定傳統 JAVA 應用程式，以將記錄傳送至 [Logz.io](https://logz.io/) 服務進行擷取和分析。 Logz.io 提供以 Elasticsearch/Logstash/Kibana (ELK) 和 Grafana 為基礎的完整監視解決方案。

本教學課程假設您使用 Log4J 或 Logback。 這兩個是以 JAVA 進行記錄時最常使用的程式庫，因此本教學課程應適用於 Azure 上執行的大部分應用程式。 如果您已經使用彈性堆疊來監視您的 JAVA 應用程式，本教學課程會示範如何重新設定為以 Logz.io 端點為目標。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 將記錄從現有 JAVA 應用程式傳送至 Logz.io。
> * 將診斷記錄和計量從 Azure 服務傳送至 Logz.io。

## <a name="prerequisites"></a>必要條件

* [Java Developer Kit](https://aka.ms/azure-jdks) 第 8 版或更高版本
* 來自 [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/logz.logzio-elk-as-a-service-pro) 的 Logz.io 帳戶
* 使用 Log4J 或 Logback 的現有 JAVA 應用程式

## <a name="send-java-application-logs-to-logzio"></a>將 JAVA 應用程式記錄傳送至 Logz.io

首先，您將了解如何使用權杖來設定您的 JAVA 應用程式，好讓應用程式能夠存取您的 Logz.io 帳戶。

### <a name="get-your-logzio-access-token"></a>取得您的 Logz.io 存取權杖

若要取得權杖，請在登入您的 Logz.io 帳戶後，選取右上角的 [齒輪] 圖示，然後選取 [設定] > [一般]  。 複製顯示在您帳戶設定中的存取權杖，以便稍後使用。

### <a name="install-and-configure-the-logzio-library-for-log4j-or-logback"></a>安裝和設定適用於 Log4J 或 Logback 的 Logz.io 程式庫

Logz.io Java 程式庫可在 Maven Central 取得，因此您可以將其新增為應用程式設定的相依性。 檢查 Maven Central 的版本號碼，並在下列組態設定中使用最新版本。

如果您使用 Maven，請將下列相依性新增至您的 `pom.xml` 檔案：

**Log4J：**

```xml
<dependency>
    <groupId>io.logz.log4j2</groupId>
    <artifactId>logzio-log4j2-appender</artifactId>
    <version>1.0.11</version>
</dependency>
```

**Logback：**

```xml
<dependency>
    <groupId>io.logz.logback</groupId>
    <artifactId>logzio-logback-appender</artifactId>
    <version>1.0.22</version>
</dependency>
```

如果您使用 Gradle，請將下列相依性新增至您的建置指令碼：

**Log4J：**

```
implementation 'io.logz.log4j:logzio-log4j-appender:1.0.11'
```

**Logback：**

```
implementation 'io.logz.logback:logzio-logback-appender:1.0.22'
```

接下來，更新您的 Log4J 或 Logback 設定檔：

**Log4J：**

```xml
<Appenders>
    <LogzioAppender name="Logzio">
        <logzioToken><your-logz-io-token></logzioToken>
        <logzioType>java-application</logzioType>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
    </LogzioAppender>
</Appenders>

<Loggers>
    <Root level="info">
        <AppenderRef ref="Logzio"/>
    </Root>
</Loggers>
```

**Logback：**

```xml
<configuration>
    <!-- Use shutdownHook so that we can close gracefully and finish the log drain -->
    <shutdownHook class="ch.qos.logback.core.hook.DelayingShutdownHook"/>
    <appender name="LogzioLogbackAppender" class="io.logz.logback.LogzioLogbackAppender">
        <token><your-logz-io-token></token>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
        <logzioType>java-application</logzioType>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
    </appender>

    <root level="debug">
        <appender-ref ref="LogzioLogbackAppender"/>
    </root>
</configuration>
```

將 `<your-logz-io-token>` 的預留位置以您的存取權杖取代，並以您的區域接聽程式主機 (例如 listener.logz.io) 取代 `<your-logz-io-listener-host>` 預留位置。 如需尋找帳戶區域的詳細資訊，請參閱[帳戶區域](https://docs.logz.io/user-guide/accounts/account-region.html)。

`logzioType` 元素會參考 Elasticsearch 中的邏輯欄位，以分隔不同的文件。 請務必適當地設定此參數，如此才能充分利用 Logz.io。

Logz.io「類型」是您的記錄格式 (例如：Apache、NGinx、MySQL)，而不是您的來源 (例如：server1、server2、server3)。 在本教學課程中，我們會呼叫 `java-application` 類型，因為我們要設定 JAVA 應用程式，而且預期這些應用程式的格式都相同。

針對進階的使用方式，您可以將 JAVA 應用程式分組成不同的類型，這些類型都有自己的特定記錄格式 (可使用 Log4J 和 Logback 來設定)。 例如，您可以有 "spring-boot-monolith" 類型和 "spring-boot-microservice" 類型。

### <a name="test-your-configuration-and-log-analysis-on-logzio"></a>在 Logz.io 上測試您的設定和記錄分析

設定好 Logz.io 程式庫之後，您的應用程式現在應該會直接將記錄傳送至此。 若要測試一切否正常運作，請移至 Logz.io 主控台，選取 [Live Tail]\(即使檢視\)  索引標籤，然後選取 [執行]  。 您應該會看到類似下面的訊息，告知您連線正常運作：

```output
Requesting Live Tail access...
Access granted. Opening connection...
Connected. Tailing...
````

接下來，啟動您的應用程式，或使用應用程式來產生一些記錄。 記錄應該會直接出現在螢幕上。 例如，以下是 Spring Boot 應用程式的第一次啟動訊息：

```output
2019-09-19 12:54:40.685Z Starting JavaApp on javaapp-default-9-5cfcb8797f-dfp46 with PID 1 (/workspace/BOOT-INF/classes started by cnb in /workspace)
2019-09-19 12:54:40.686Z The following profiles are active: prod
2019-09-19 12:54:42.052Z Bootstrapping Spring Data repositories in DEFAULT mode.
2019-09-19 12:54:42.169Z Finished Spring Data repository scanning in 103ms. Found 6 repository interfaces.
2019-09-19 12:54:43.426Z Bean 'spring.task.execution-org.springframework.boot.autoconfigure.task.TaskExecutionProperties' of type [org.springframework.boot.autoconfigure.task.TaskExecutionProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
```

現在，您的記錄會經由 Logz.io 處理，您可以開始從所有平台服務中獲益。

## <a name="send-azure-services-data-to-logzio"></a>將 Azure 服務資料傳送至 Logz.io

接下來，您將了解如何將記錄和計量從 Azure 資源傳送至 Logz.io。

### <a name="deploy-the-template"></a>部署範本

第一個步驟是部署 Logz.io - Azure 整合範本。 此整合是以現成的 Azure 部署範本為基礎，其中可設定管線的所有必要構成要素。 此範本會建立事件中樞命名空間、事件中樞、兩個儲存體 Blob，以及所有必要的正確權限和連線。 自動部署所設定的資源可以收集單一 Azure 區域的資料，並將該資料傳送至 Logz.io。

尋找[存放庫讀我檔案第一個步驟](https://github.com/logzio/logzio-azure-serverless)中顯示的 [部署至 Azure]  按鈕。

當您選取 [部署至 Azure]  時，Azure 入口網站中的 [自訂部署]  頁面會顯示預先填入的欄位清單。

您可以將大部分的欄位保持原狀，但請務必輸入下列設定：

* **資源群組**：選取現有群組或建立新的群組。
* **Logzio 記錄/計量主機**：輸入 Logz.io 接聽程式的 URL。 如果您不確定此 URL 是什麼，請查看您的登入 URL。 如果是 app.logz.io，請使用 listener.logz.io (這是預設設定)。 如果是 app-eu.logz.io，請使用 listener-eu.logz.io。
* **Logzio 記錄/計量權杖**：輸入 Logz.io 帳戶的權杖，以將 Azure 記錄或計量傳送至該帳戶。 您可以在 Logz.io UI 的帳戶頁面上找到此權杖。

同意頁面底部的條款，然後選取 [購買]  。 接著，Azure 會開始部署範本，這可能需要一或兩分鐘的時間。 最後，您會在入口網站頂端看到「部署成功」的訊息。

您可以瀏覽定義的資源群組，以檢閱已部署的資源。

若要了解如何設定 logzio-azure-serverless 來將資料備份至 Azure Blob 儲存體，請參閱[ Azure 活動記錄](https://docs.logz.io/shipping/log-sources/azure-activity-logs.html)。

### <a name="stream-azure-logs-and-metrics-to-logzio"></a>將 Azure 記錄和計量串流至 Logz.io

現在您已部署整合範本，接著必須將 Azure 設定為將診斷資料串流至您剛部署好的事件中樞。 當資料進入事件中樞時，函式應用程式會接著將該資料轉送至 Logz.io。

1. 在搜尋列中輸入「診斷」，然後選取 [診斷設定]  。

2. 從資源清單中選擇資源，然後選取 [新增診斷設定]  來開啟該資源的 [診斷設定]  面板。

    ![診斷設定面板](media/java-get-started-with-logzio/diagnostics-settings.png)

3. 指定診斷設定的 [名稱]  。

4. 選取 [串流到事件中樞]  ，然後選取 [設定]  以開啟 [選取事件中樞]  面板。

5. 選擇事件中樞：

    * **選取事件中樞命名空間**：選擇開頭為 **Logzio** 的命名空間 (例如 `LogzioNS6nvkqdcci10p`)。
    * **選取事件中樞名稱**：針對 [記錄]，請選擇 [insights-operational-logs]  ，然後選擇 [insights-operational-metrics]  作為 [計量]。
    * **選取事件中樞原則名稱**：選擇 [LogzioSharedAccessKey]  。

6. 選取 [確定]  以返回 [診斷設定]  面板。

7. 在 [記錄] 區段中，選取您想要串流的資料，然後選取 [儲存]  。

選取的資料現在會串流至事件中樞。

### <a name="visualize-your-data"></a>將您的資料視覺化

接下來，請等候一段時間，讓資料從您的系統傳送至 Logz.io，然後開啟 Kibana。 您應該會看到儀表板中已填入資料 (類型為 eventhub  )。 如需如何建立儀表板的詳細資訊，請參閱[建立完美的 Kibana 儀表板](https://logz.io/blog/perfect-kibana-dashboard/)。

您可以在儀表板的 [探索]  索引標籤中查詢特定資料，或在 [視覺化]  標籤上建立 Kibana 物件來將資料視覺化。

## <a name="clean-up-resources"></a>清除資源

當您完成使用本教學課程中建立的 Azure 資源時，您可以使用下列命令將其刪除：

```azurecli-interactive
az group delete --name <resource group>
```

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何設定 JAVA 應用程式和 Azure 服務來將記錄和計量傳送至 Logz.io。

接下來，您可以深入了解如何使用事件中樞來監視應用程式：

> [!div class="nextstepaction"]
> [將 Azure 監視資料串流至事件中樞以供外部工具取用](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)
