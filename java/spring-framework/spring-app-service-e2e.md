---
title: 使用適用於 MySQL 的 Azure 資料庫將 Spring/Tomcat 應用程式部署至 App Service
description: 使用 MySQL 進行 Java App Service 的端對端教學課程
author: KarlErickson
manager: barbkess
ms.author: karler
ms.date: 11/12/2019
ms.service: app-service
ms.devlang: java
ms.topic: article
ms.openlocfilehash: 2f5fc593733ea1f90d87ef9fbf98d6656410fa66
ms.sourcegitcommit: 25cef39b178a175822bf29f28fb2658bb8df8c59
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2019
ms.locfileid: "74040533"
---
# <a name="deploy-a-spring-app-to-app-service-with-mysql"></a>使用 MySQL 將 Spring 應用程式部署至 App Service

本教學課程會引導您完成在 App Service Linux 中建置、設定、部署、疑難排解及調整 Java Web 應用程式的程序。

本教學課程是以熱門的 Spring PetClinic 範例應用程式為基礎。 在本主題中，您將在本機測試應用程式的 HSQLDB 版本，然後將它部署到 [Azure App Service](/azure/app-service/containers)。 之後，您將設定和部署使用[適用於 MySQL 的 Azure 資料庫](/azure/mysql)的版本。 最後，您將了解如何存取應用程式記錄，以及藉由增加執行您應用程式的背景工作角色數目來相應放大。

## <a name="prerequisites"></a>必要條件

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview)
* [Java 8](http://java.oracle.com/)
* [Maven 3](http://maven.apache.org/)
* [Git](https://github.com/)
* [Tomcat](https://tomcat.apache.org/download-80.cgi)
* [MySQL CLI](http://dev.mysql.com/downloads/mysql/)

## <a name="get-the-sample"></a>取得範例

若要開始使用範例應用程式，請使用下列命令來複製及準備來源存放庫。

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux.git
cd e2e-java-experience-in-app-service-linux
yes | cp -rf .prep/* .
```

## <a name="build-and-run-the-hsqldb-sample-locally"></a>在本機建置和執行 HSQLDB 範例

首先，我們將使用 HSQLDB 作為資料庫，在本機測試範例。

瀏覽至 HSQLDB 版本的範例，然後加以建置。

```bash
cd initial-hsqldb/spring-framework-petclinic
mvn package
```

接著，將 TOMCAT_HOME 環境變數設定為 Tomcat 安裝的位置。

```bash
export TOMCAT_HOME=<Tomcat install directory>
```

然後，更新 *pom.xml* 檔案以針對 Tomcat WAR 檔案部署設定 Maven。 新增下列 XML 作為現有 `<plugins>` 元素的子節點。 如有必要，請將 `1.7.7` 變更為最新版的 [Cargo Maven 2 外掛程式](https://mvnrepository.com/artifact/org.codehaus.cargo/cargo-maven2-plugin)。

```xml
<plugin>
    <groupId>org.codehaus.cargo</groupId>
    <artifactId>cargo-maven2-plugin</artifactId>
    <version>1.7.7</version>
    <configuration>
        <container>
            <containerId>tomcat8x</containerId>
            <type>installed</type>
            <home>${TOMCAT_HOME}</home>
        </container>
        <configuration>
            <type>existing</type>
            <home>${TOMCAT_HOME}</home>
        </configuration>
        <deployables>
            <deployable>
                <groupId>${project.groupId}</groupId>
                <artifactId>${project.artifactId}</artifactId>
                <type>war</type>
                <properties>
                    <context>/</context>
                </properties>
            </deployable>
        </deployables>
    </configuration>
</plugin>
```

完成此設定之後，您就可以在本機將應用程式部署到 Tomcat。

```bash
mvn cargo:deploy
```

然後啟動 Tomcat。

```bash
${TOMCAT_HOME}/bin/catalina.sh run
```

您現在可以將瀏覽器導覽至 [http://localhost:8080](http://localhost:8080)，以查看執行中的應用程式並了解其運作方式。 當您完成時，請在 Bash 提示字元選取 Ctrl+C 來停止 Tomcat。

## <a name="deploy-to-azure-app-service"></a>部署到 Azure App Service

既然您已看到它在本機執行，我們會將應用程式部署至 Azure。

首先，設定下列環境變數。

```bash
export RESOURCEGROUP_NAME=<resource group>
export WEBAPP_NAME=<web app>
export WEBAPP_PLAN_NAME=${WEBAPP_NAME}-appservice-plan
export REGION=<region>
```

Maven 將會使用這些值，以您提供的名稱建立 Azure 資源。 藉由使用環境變數，您可將您的帳戶秘密保存在專案檔案之外。

然後，更新 *pom.xml* 檔案以針對 Azure 部署設定 Maven。 在您先前新增的 `<plugin>` 元素後面新增下列 XML。 如有必要，請將 `1.7.0` 變更為最新版的 [Maven 外掛程式 (適用於 Azure App Service)](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)。

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.8.0</version>
    <configuration>
        <schemaVersion>v2</schemaVersion>
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>
        <runtime>
            <os>linux</os>
            <javaVersion>jre8</javaVersion>            
            <webContainer>TOMCAT 8.5</webContainer>
        </runtime>
    </configuration>
</plugin>
```

接著登入 Azure。

```azurecli
az login
```

然後將應用程式部署到 App Service Linux。

```bash
mvn azure-webapp:deploy
```

您現在可以瀏覽至 `https://<app-name>.azurewebsites.net` (在取代`<app-name>` 之後) 以查看執行中的應用程式。

## <a name="set-up-azure-database-for-mysql"></a>設定適用於 MySQL 的 Azure 資料庫

接著，我們會切換為使用 MySQL，而不是 HSQLDB。 我們會在 Azure 上建立 MySQL 伺服器執行個體並新增資料庫，然後使用新的資料庫連線資訊來更新應用程式組態。

首先，設定下列環境變數以便在稍後的步驟中使用。

```bash
export MYSQL_SERVER_NAME=<server>
export MYSQL_SERVER_FULL_NAME=${MYSQL_SERVER_NAME}.mysql.database.azure.com
export MYSQL_SERVER_ADMIN_LOGIN_NAME=<admin>
export MYSQL_SERVER_ADMIN_PASSWORD=<password>
export MYSQL_DATABASE_NAME=<database>
export DOLLAR=\$
```

接著，建立並初始化資料庫伺服器。 使用 [az mysql up](/cli/azure/ext/db-up/mysql?view=azure-cli-latest#ext-db-up-az-mysql-up) 進行初始設定。 然後使用 [az mysql server configuration set](/cli/azure/mysql/server/configuration?view=azure-cli-latest#az-mysql-server-configuration-set)來增加連線逾時並設定伺服器時區。

```azurecli
az mysql up \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server-name ${MYSQL_SERVER_NAME} \
    --database-name ${MYSQL_DATABASE_NAME} \
    --admin-user ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
    --admin-password ${MYSQL_SERVER_ADMIN_PASSWORD}

az mysql server configuration set --name wait_timeout \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value 2147483

az mysql server configuration set --name time_zone \
    --resource-group ${RESOURCEGROUP_NAME} \
    --server ${MYSQL_SERVER_NAME} --value -8:00
```

然後，使用 MySQL CLI 來建立資料庫。

```bash
mysql -u ${MYSQL_SERVER_ADMIN_LOGIN_NAME} \
 -h ${MYSQL_SERVER_FULL_NAME} -P 3306 -p
```

在 MySQL CLI 提示字元中，執行下列命令，並將 `<database name>`取代為您先前為 `MYSQL_DATABASE_NAME` 環境變數指定的相同值。

```console
CREATE DATABASE <database name>;
```

MySQL 現已可供使用。

## <a name="configure-the-app-for-mysql"></a>設定適用於 MySQL 的應用程式

接下來，我們會將連線資訊新增至 MySQL 版本的應用程式，然後將其部署至 App Service。

首先，在 Bash 提示字元瀏覽至正確的資料夾。

```bash
cd ../../initial-mysql/spring-framework-petclinic
```

然後，更新 *pom .xml* 檔案，讓 MySQL 成為作用中的組態。 從 HSQLDB 設定檔中移除 `<activation>` 元素，並改為將它放在 MySQL 設定檔中，如下所示。 程式碼片段的其餘部分會顯示現有的組態。 請注意，Maven 如何使用您先前設定的環境變數來設定您的 MySQL 存取。

```xml
<profile>
    <id>MySQL</id>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    <properties>
        <db.script>mysql</db.script>
        <jpa.database>MYSQL</jpa.database>
        <jdbc.driverClassName>com.mysql.jdbc.Driver</jdbc.driverClassName>
        <jdbc.url>jdbc:mysql://${DOLLAR}{MYSQL_SERVER_FULL_NAME}:3306/${DOLLAR}{MYSQL_DATABASE_NAME}?useUnicode=true</jdbc.url>
        <jdbc.username>${DOLLAR}{MYSQL_SERVER_ADMIN_LOGIN_NAME}</jdbc.username>
        <jdbc.password>${DOLLAR}{MYSQL_SERVER_ADMIN_PASSWORD}</jdbc.password>
    </properties>
    ...
</profile>
```

然後，更新 *pom.xml* 檔案以針對 Azure 部署和 MySQL 使用設定 Maven。 在您先前新增的 `<plugin>` 元素後面新增下列 XML。 如有必要，請將 `1.7.0` 變更為最新版的 [Maven 外掛程式 (適用於 Azure App Service)](/java/api/overview/azure/maven/azure-webapp-maven-plugin/readme)。

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>

        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appServicePlanName>${WEBAPP_PLAN_NAME}</appServicePlanName>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>

        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>

        <appSettings>
            <property>
                <name>MYSQL_SERVER_FULL_NAME</name>
                <value>${MYSQL_SERVER_FULL_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_LOGIN_NAME</name>
                <value>${MYSQL_SERVER_ADMIN_LOGIN_NAME}</value>
            </property>
            <property>
                <name>MYSQL_SERVER_ADMIN_PASSWORD</name>
                <value>${MYSQL_SERVER_ADMIN_PASSWORD}</value>
            </property>
            <property>
                <name>MYSQL_DATABASE_NAME</name>
                <value>${MYSQL_DATABASE_NAME}</value>
            </property>
        </appSettings>

    </configuration>
</plugin>
```

接著，建置應用程式，然後透過 Tomcat 部署並執行它，以在本機進行測試。

```bash
mvn package
mvn cargo:deploy
${TOMCAT_HOME}/bin/catalina.sh run
```

您現在可以在 [http://localhost:8080](http://localhost:8080) 本機檢視應用程式。 應用程式的外觀和行為與之前相同，但使用適用於 MySQL 的 Azure 資料庫而不是 HSQLDB。 當您完成時，請在 Bash 提示字元選取 Ctrl+C 來停止 Tomcat。

最後，將應用程式部署到 App Service。

```bash
mvn azure-webapp:deploy
```

您現在可以瀏覽至 `https://<app-name>.azurewebsites.net`，使用 App Service 和適用於 MySQL 的 Azure 資料庫查看執行中的應用程式。

## <a name="access-the-app-logs"></a>存取應用程式記錄

如果您需要進行疑難排解，可以查看應用程式記錄。 若要在本機電腦上開啟遠端記錄資料流，請使用下列命令。

```azurecli
az webapp log tail --name ${WEBAPP_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

當您檢視完記錄時，請選取 Ctrl+C 來停止資料流。

記錄資料流也可在 `https://<app-name>.scm.azurewebsites.net/api/logstream` 取得。

## <a name="scale-out"></a>相應放大

若要支援您應用程式增加的流量，您可使用下列命令相應放大為多個執行個體。

```azurecli
az appservice plan update --number-of-workers 2 \
    --name ${WEBAPP_PLAN_NAME} \
    --resource-group ${RESOURCEGROUP_NAME}
```

恭喜！ 您使用 Spring Framework、JSP、Spring Data、Hibernate、JDBC、App Service Linux 和適用於 MySQL 的 Azure 資料庫建置了 Java Web 應用程式並相應放大。

## <a name="clean-up-resources"></a>清除資源

在前述各節中，您在資源群組中建立了 Azure 資源。 如果您在未來不需使用這些資源，請執行下列命令來刪除資源群組。

```azurecli
az group delete --name ${RESOURCEGROUP_NAME}
```

## <a name="next-steps"></a>後續步驟

接著，請查看可供 Java 與 App Service 使用的其他組態和 CI/CD 選項。

> [!div class="nextstepaction"]
> [為 Azure App Service 設定 Linux Java 應用程式](/azure/app-service/containers/configure-language-java)
> [!div class="nextstepaction"]
> [使用 Azure Pipelines 建置和部署到 Java Web 應用程式](/azure/devops/pipelines/ecosystems/java-webapp?view=azure-devops&tabs=java-tomcat)
> [!div class="nextstepaction"]
> [使用 Jenkins 外掛程式部署到 Azure App Service](/azure/jenkins/deploy-jenkins-app-service-plugin)
