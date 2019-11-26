---
title: 透過 Maven 將 Spring Boot JAR 檔案應用程式部署到 Azure
description: 了解如何使用適用於 Azure Web App for Linux 的 Maven 外掛程式，將 Spring Boot 應用程式部署至雲端。
services: app-service
documentationcenter: java
author: bmitchell287
manager: douge
editor: brborges
ms.author: brendm
ms.date: 12/19/2018
ms.devlang: java
ms.service: app-service
ms.topic: article
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 274287a6e29bd75721758805d508ebebfcc7586a
ms.sourcegitcommit: 8be617e100ae3d3e90d56c672b1c7c110b7a588f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2019
ms.locfileid: "74160736"
---
# <a name="deploy-a-spring-boot-jar-file-app-to-azure-app-service-with-maven-and-azure-on-linux"></a>透過 Linux 上的 Maven 和 Azure 將 Spring Boot JAR 檔案應用程式部署至 Azure App Service

在本快速入門中，您會使用使用[適用於 App Service Web Apps 的 Maven 外掛程式](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)，將 Spring Boot 應用程式封裝成 Java SE JAR，並部署至 [Linux 上的 Azure App Service](/azure/app-service/containers/)。 若要將應用程式相依性、執行階段及組態合併至單一可部署成品，您可從 [Tomcat 與 WAR 檔案](/azure/app-service/containers/quickstart-java)中選擇 Java SE 部署。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程中的步驟，您必須已安裝以下項目並設定完成：

* [Azure CLI](/cli/azure/)，安裝在本機或透過 [Azure Cloud Shell](https://shell.azure.com)安裝。
* 受支援的 Java 開發套件 (JDK)。 如需在 Azure 上進行開發時可使用的 JDK 相關資訊，請參閱 <https://aka.ms/azure-jdks>。
* Apache 的 [Maven](https://maven.apache.org/) 3.0 版。
* [Git](https://git-scm.com/downloads) 用戶端。

## <a name="install-and-sign-in-to-azure-cli"></a>安裝並登入 Azure CLI

讓 Maven 外掛程式部署 Spring Boot 應用程式最簡單且最輕鬆的方式是使用 [Azure CLI](/cli/azure/)。

使用 Azure CLI 登入您的 Azure 帳戶：
   
   ```shell
   az login
   ```
   
依照指示完成登入程序。

## <a name="clone-the-sample-app"></a>複製範例應用程式

在本節中，您會在本機複製已完成的 Spring Boot 應用程式，並且進行測試。

1. 開啟命令提示字元或終端機視窗，並建立本機目錄來保存您的 Spring Boot 應用程式，然後變更至該目錄；例如：
   ```shell
   md C:\SpringBoot
   cd C:\SpringBoot
   ```
   -- 或 --
   ```shell
   mkdir ~/SpringBoot
   cd ~/SpringBoot
   ```

1. 將 [Spring Boot Getting Started] 範例專案複製到您建立的目錄中；例如：
   ```shell
   git clone https://github.com/spring-guides/gs-spring-boot
   ```

1. 將目錄變更至已完成的專案；例如：
   ```shell
   cd gs-spring-boot/complete
   ```

1. 使用 Maven 建立 JAR 檔案；例如：
   ```shell
   mvn clean package
   ```

1. 建立 Web 應用程式後，使用 Maven 啟動 Web 應用程式，例如：
   ```shell
   mvn spring-boot:run
   ```

1. 測試 Web 應用程式，方法是使用網頁瀏覽器在本機瀏覽它。 例如，如果您有 curl 可用，可以使用下列命令：
   ```shell
   curl http://localhost:8080
   ```

1. 您應該會看到下列訊息：**Greetings from Spring Boot!**

## <a name="configure-maven-plugin-for-azure-app-service"></a>為 Azure App Service 設定 Maven 外掛程式

在本節中，您將設定 Spring Boot 專案`pom.xml`，以便 Maven 可以將應用程式部署至 Linux 上的 Azure App Service。

1. 在程式碼編輯器中，開啟 `pom.xml`。

2. 在 pom.xml 的 `<build>` 區段中，於 `<plugins>` 標記內新增下列 `<plugin>` 項目。

   ```xml
   <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.8.0</version>
   </plugin>
   ```

3. 接著您可以設定部署、在命令提示字元中執行下列 Maven 命令，並使用**數字**在提示中選擇這些選項：
    * **OS**：Linux  
    * **javaVersion**：Java 8    
    
   ```cmd
   mvn azure-webapp:config
   ```

   看到 **Confirm (Y/N)** 提示時，按下 **'y'** 完成設定。

   ```cmd
   ~@Azure:~/gs-spring-boot/complete$ mvn azure-webapp:config
   [INFO] Scanning for projects...
   [INFO]
   [INFO] -----------------< org.springframework:gs-spring-boot >-----------------
   [INFO] Building gs-spring-boot 0.1.0
   [INFO] --------------------------------[ jar ]---------------------------------
   [INFO]
   [INFO] --- azure-webapp-maven-plugin:1.6.0:config (default-cli) @ gs-spring-boot ---
   [WARNING] The plugin may not work if you change the os of an existing webapp.
   Define value for OS(Default: Linux):
   1. linux [*]
   2. windows
   3. docker
   Enter index to use:
   Define value for javaVersion(Default: Java 8):
   1. Java 11
   2. Java 8 [*]
   Enter index to use:
   Please confirm webapp properties
   AppName : gs-spring-boot-1559091271202
   ResourceGroup : gs-spring-boot-1559091271202-rg
   Region : westeurope
   PricingTier : Premium_P1V2
   OS : Linux
   RuntimeStack : JAVA 8-jre8
   Deploy to slot : false
   Confirm (Y/N)? : Y
   ```

4. 將 `<appSettings>` 區段新增到 `<azure-webapp-maven-plugin>` 的 `<configuration>` 區段，以接聽連接埠 *80*。

   ```xml
   <plugin>
       <groupId>com.microsoft.azure</groupId>
       <artifactId>azure-webapp-maven-plugin</artifactId>
       <version>1.8.0</version>
       <configuration>
          <schemaVersion>V2</schemaVersion>
          <resourceGroup>gs-spring-boot-1559091271202-rg</resourceGroup>
          <appName>gs-spring-boot-1559091271202</appName>
          <region>westeurope</region>
          <pricingTier>P1V2</pricingTier>
          <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>
            <webContainer>jre8</webContainer>
          </runtime>
          <!-- Begin of App Settings  -->
          <appSettings>
             <property>
                   <name>JAVA_OPTS</name>
                   <value>-D server.port=80</value>
             </property>
          </appSettings>
          <!-- End of App Settings  -->
          <deployment>
            <resources>
              <resource>
                <directory>${project.basedir}/target</directory>
                <includes>
                  <include>*.jar</include>
                </includes>
              </resource>
            </resources>
          </deployment>
         </configuration>
   </plugin>
   ```

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

一旦您已設定本文上述章節中的所有設定，您已準備好將 Web 應用程式部署至 Azure。 若要這樣做，請使用下列步驟：

1. 如果您對 pom.xml  檔案進行任何變更，從您稍早使用的命令提示字元或終端機視窗，使用 Maven 重新建置 JAR 檔案；例如：
   ```shell
   mvn clean package
   ```

1. 使用 Maven 將您的 Web 應用程式部署至 Azure；例如：
   ```shell
   mvn azure-webapp:deploy
   ```

Maven 會將您的 Web 應用程式部署至 Azure；如果 Web 應用程式或 Web 應用程式方案不存在，系統會為您建立。 可能需要幾分鐘的時間，Web 應用程式才會顯示在輸出中的 URL 上。 請在網頁瀏覽器中瀏覽至 URL。  您應該會看到顯示下列內容的訊息：Greetings from Spring Boot!

網站部署完成時，您就可以使用 [Azure 入口網站]來管理。

* 您的 Web 應用程式會列在**應用程式服務** 中：

   ![列在 Azure 入口網站應用程式服務中的 Web 應用程式][AP01]

* Web 應用程式的 URL 會列在 Web 應用程式的 [概觀]  中：

   ![在 Azure 入口網站應用程式服務中尋找 Web 應用程式的 URL][AP02]

使用與之前相同的 cURL 命令來確認部署是否成功；請使用入口網站中的 Web 應用程式 URL，不要使用 `localhost`。 您應該會看到下列訊息：**Greetings from Spring Boot!** 

## <a name="clean-up-resources"></a>清除資源
不再需要 Azure 資源時，可藉由刪除資源群組來清除您所部署的資源。

- 在 Azure 入口網站中，選取左側功能表中的 [資源群組]。
- 在 [依名稱篩選]  欄位中輸入 **gs-spring-boot-** ，在本教學課程中建立的資源群組應該具有此前置詞。
- 在本教學課程中選取資源群組。
- 從頂端功能表中選取 [刪除資源群組]。

## <a name="next-steps"></a>後續步驟

若要深入了解 Spring 和 Azure，請繼續閱讀「Azure 上的 Spring」文件中心中的資訊。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/java/spring-framework)

### <a name="additional-esources"></a>其他資源

如需本文所討論之各種技術的詳細資訊，請參閱下列文章：

* [適用於 Azure Web 應用程式的 Maven 外掛程式]

* [如何使用適用於 Azure Web 應用程式的 Maven 外掛程式，將容器化 Spring Boot 應用程式部署至 Azure](deploy-containerized-spring-boot-java-app-with-maven-plugin.md)

* [使用 Azure CLI 2.0 來建立 Azure 服務主體](/cli/azure/create-an-azure-service-principal-azure-cli)

* [Maven 設定參考](https://maven.apache.org/settings.html)

<!-- URL List -->

[Azure Command-Line Interface (CLI)]: /cli/azure/overview
[Azure for Java Developers]: /azure/java/
[Azure 入口網站]: https://portal.azure.com/
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Git]: https://github.com/
[Working with Azure DevOps and Java]: /azure/devops/
[Maven]: http://maven.apache.org/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot Getting Started]: https://github.com/spring-guides/gs-spring-boot
[Spring Framework]: https://spring.io/
[適用於 Azure Web 應用程式的 Maven 外掛程式]: /java/api/overview/azure/maven/azure-webapp-maven-plugin/readme

[Java Development Kit (JDK)]: https://aka.ms/azure-jdks
<!-- http://www.oracle.com/technetwork/java/javase/downloads/ -->

<!-- IMG List -->


[AP01]: ./media/deploy-spring-boot-java-app-with-maven-plugin/web-app-listed-azure-portal.png
[AP02]: ./media/deploy-spring-boot-java-app-with-maven-plugin/determine-web-app-url.png
