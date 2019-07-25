---
title: 搭配使用 Docker 映像與適用於 Azure Java 開發的 JDK
description: ''
author: bmitchell287
manager: douge
ms.author: brendm
ms.date: 04/09/2019
ms.devlang: java
ms.topic: conceptual
ms.service: Azure
ms.openlocfilehash: 5de2866f1f303deccf40158ac7451dbd35de92f1
ms.sourcegitcommit: 4cc7f5e1e4601065bfcb4c2eeb7d47ad0bec61f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68430953"
---
# <a name="use-docker-with-a-jdk-for-azure"></a>搭配使用 Docker 與適用於 Azure 的 JDK 

針對 Java 7、8 和 11 預先建置的 Docker 映像可透過 [Docker Hub](https://hub.docker.com/_/microsoft-java-se) 取得。

多基底 OS 映像上已針對 Zulu JDK、JRE 和無周邊 JRE 通過認證的 Docker 容器映像，可從 Docker Hub 取得：

* [JDK](https://hub.docker.com/_/microsoft-java-jdk)
* [JRE](https://hub.docker.com/_/microsoft-java-jre)
* [無周邊 JRE](https://hub.docker.com/_/microsoft-java-jre-headless)

## <a name="running-a-docker-image"></a>執行 Docker 映像

Docker 映像可以使用 `$ docker run mcr.microsoft.com/java/jdk:tag java` 語法來執行，如下列範例所示。

```cli
docker run mcr.microsoft.com/java/jdk:8u212-zulu-alpine java -version 
```

## <a name="creating-a-docker-image"></a>建立 Docker 映像

您可以使用 Microsoft 的官方 Docker Hub 映像來建立映像，如下列範例所示。

### <a name="create-a-docker-file"></a>建立 Docker 檔案

```cli
FROM mcr.microsoft.com/java/jdk:8u212-zulu-alpine 
  
RUN echo $' \
  
public class HelloWorld { \
   public static void main(String[] args) { \
      // Prints "Hello, World" in the terminal window. \
      System.out.println("Hello, World - From Microsoft Azure !!!"); \
   } \
}' > HelloWorld.java
  
RUN javac HelloWorld.java
  
CMD ["java", "HelloWorld"]
```

### <a name="build-a-docker-image"></a>建置 Docker 映像

```cli
docker build -t hello-world
```

### <a name="run-the-new-image"></a>執行新的映像

```cli
docker run hello-world
```

您會看到下列輸出︰

```output
Hello World - From Microsoft Azure !!!
```
