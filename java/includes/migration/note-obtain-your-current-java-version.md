---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 2fc4e3b43a051103e7aea6aa77f66666ca454910
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825864"
---
<!-- Included in "### Switch to a supported platform" sections that have different (required) intro paragraphs. For example:

### Switch to a supported platform

App Service offers specific versions of Java SE. To ensure compatibility, migrate your application to one of the supported versions of in its current environment before you proceed with any of the remaining steps. Be sure to fully test the resulting configuration. Use the latest stable release of your Linux distribution in such tests.

-->

> [!NOTE]
> 如果目前的伺服器是在不支援的 JDK (例如 Oracle JDK 或 IBM OpenJ9) 上執行，這項驗證就特別重要。

若要取得目前的 Java 版本，請登入您的實際執行伺服器，然後執行下列命令：

```bash
java -version
```

若要取得 Azure App Service 所使用的目前版本，請下載 [Zulu 8](https://www.azul.com/downloads/azure-only/zulu/?&version=java-8-lts&architecture=x86-64-bit&package=jdk) (如果您想要使用 Java 8 執行階段)，或下載 [Zulu 11](https://www.azul.com/downloads/azure-only/zulu/?&version=java-11-lts&architecture=x86-64-bit&package=jdk) (如果您想要使用 Java 11 執行階段)。
