---
title: 將應用程式遷移至 Azure
description: 本主題提供將 Java 應用程式遷移至 Azure 的建議策略概觀。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.openlocfilehash: d32c38d763901152135b965484362031dfac7f0a
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825792"
---
# <a name="migrate-java-applications-to-azure"></a>將應用程式遷移至 Azure

本主題提供將 Java 應用程式遷移至 Azure 的建議策略概觀。

## <a name="identifying-application-type"></a>識別應用程式類型

在您為 Java 應用程式選取雲端目的地之前，您必須先識別其應用程式類型。 大部分的 Java 應用程式都是下列其中一種類型：

* [Spring Boot / JAR 應用程式](#spring-boot--jar-applications)
* [Spring Cloud / 微服務](#spring-cloud--microservices)
* [Web 應用程式](#web-applications)
* [Java EE 應用程式](#java-ee-applications)
* [批次 / 排程作業](#batch--scheduled-jobs)

這些類型將於下列各節中說明。

### <a name="spring-boot--jar-applications"></a>Spring Boot / JAR 應用程式

許多較新的應用程式都是從命令列直接叫用。 這些應用程式仍會處理 Web 要求，但不是依賴應用程式伺服器來提供 HTTP 要求處理，而是它們會將 HTTP 通訊和所有其他相依性直接納入應用程式封裝中。 這類應用程式通常是以像是 Spring Boot、Dropwizard、Micronaut、MicroProfile、x.x.x.x 等等架構來組建的。

這些應用程式會封裝成副檔名為 *.jar* 的封存 (JAR 檔案)。

### <a name="spring-cloud--microservices"></a>Spring Cloud / 微服務

微服務架構樣式是一種將單一應用程式開發成一套小型服務的方法，每個小型服務都在自己的處理序中執行，並與輕量機制 (通常是 HTTP 資源 API) 通訊。 這些服務是根據商務功能來組建的，並可由完全自動化的部署機制獨立部署。 幾乎沒有任何方法來集中管理這些服務，它們可能是以不同的程式設計語言來撰寫的，並使用不同的資料儲存技術。 這類服務通常是以 Spring Cloud 這類架構組建的。

這些服務會封裝成多個副檔名為 *.jar* 的應用程式 (JAR 檔案)。

### <a name="web-applications"></a>Web 應用程式

Web 應用程式會在 [Servlet](https://en.wikipedia.org/wiki/Java_servlet) 容器內執行。 有些應用程式會直接使用 servlet Api，而許多應用程式則會使用封裝 servlet API 的其他架構，例如 Apache Struts、Spring MVC、JavaServer、Faces (JSF)，以及其他。

Web 應用程式會封裝成副檔名為 *.war* 的封存 (WAR 檔案)。

### <a name="java-ee-applications"></a>Java EE 應用程式

JAVA EE 應用程式 (也稱為 J2EE 應用程式，或最近稱為 JakartaEE 應用程式) 可以包含 Web 應用程式的部分、全部元素，或完全沒有。 它們也可以包含和取用許多其他元件，如 [Java EE 規格](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition)所定義的元件。

Java EE 應用程式可以封裝成副檔名為 *.ear* 的封存 (EAR 檔案)，或封裝成副檔名為 *.war* 的封存 (WAR 檔案)。

JAVA EE 應用程式必須部署至與 Java EE 相容的應用程式伺服器 (例如 WebLogic、WebSphere、WildFly、GlassFish、Payara 和其他)。

應用程式 (亦即，與應用程式伺服器無關的應用程式) 若僅依賴 Java EE 規格所提供的功能，可以從某個相容的應用程式伺服器遷移至另一個。 如果您的應用程式依賴特定的應用程式伺服器 (與應用程式伺服器相關)，您可能需要選取可讓您裝載該應用程式伺服器的 Azure 服務目的地。

### <a name="batch--scheduled-jobs"></a>批次 / 排程作業

某些應用程式旨在短暫執行、執行特定工作負載，然後結束，而不是等待要求或使用者輸入。 有時候，這類作業需要執行一次或定期執行。 在內部部署中，這類作業通常是從伺服器的 crontab 叫用。

這些應用程式會封裝成副檔名為 *.jar* 的封存 (JAR 檔案)。

> [!NOTE]
> 如果您的應用程式使用排程器 (例如 Spring Batch 或 Quartz) 來執行排程工作，強烈建議您納入這類工作，以在應用程式外部執行。 如果您的應用程式擴展為雲端中的多個執行個體，則相同的作業會執行多次。 此外，如果您的排程機制使用主機的當地時區，則在跨區域擴展您的應用程式時，您可能會遇到不需要的行為。

## <a name="selecting-the-target-azure-service-destination"></a>選取目標 Azure 服務目的地

下列各節將為您展示哪些服務目的地符合您的應用程式需求，以及它們牽涉到哪些責任。

### <a name="feature-grid"></a>功能方格

使用下列方格來識別支援所需應用程式類型和功能的目的地。

|   |App<br>服務<br>Java SE|App<br>服務<br>Tomcat|App<br>服務<br>WildFly|Azure<br>Spring<br>Cloud|AKS|虛擬機器|
|---|---|---|---|---|---|---|
| Spring Boot / JAR 應用程式                                    |&#x2714;|        |        |        |&#x2714;|&#x2714;|
| Spring Cloud / 微服務                                      |        |        |        |&#x2714;|&#x2714;|&#x2714;|
| Web 應用程式                                                  |        |&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| Java EE 應用程式                                              |        |        |&#x2714;|        |&#x2714;|&#x2714;|
| 商務應用程式伺服器<br>(例如 WebLogic 或 WebSphere) |        |        |        |        |&#x2714;|&#x2714;|
| 長期保存在本機檔案系統上                         |&#x2714;|&#x2714;|&#x2714;|        |&#x2714;|&#x2714;|
| 應用程式伺服器層級叢集                               |        |        |        |        |&#x2714;|&#x2714;|
| 批次 / 排程作業                                            |        |        |        |&#x2714;|&#x2714;|&#x2714;|

### <a name="ongoing-responsibility-grid"></a>持續責任方格

使用下列方格來了解在遷移後每個目的地加諸在您小組的責任。

您的小組會持續負責以 "&#x1F449;" 表示的工作。 建議您實作健全且高度自動化的處理序，以履行所有這類責任。 

> [!NOTE]
> 這不是完整的責任清單。

|   | App Service 方案 | Azure Spring Cloud | AKS | 虛擬機器 |
|---|---|---|---|---|
| 更新程式庫<br>(包括弱點補救)                 | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |
| 更新應用程式伺服器<br>(包括弱點補救)    | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 更新 Java 執行階段<br>(包括弱點補救)          | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 觸發 Kubernetes 更新<br>(由 Azure 搭配手動觸發程序執行) | N/A         | N/A         | &#x1F449;   | N/A       |
| 協調與舊版不相容的 Kubernetes API 變更                  | N/A         | N/A         | &#x1F449;   | N/A       |
| 更新容器基礎映像<br>(包括弱點補救)      | N/A         | N/A         | &#x1F449;   | N/A       |
| 更新作業系統<br>(包括弱點補救)      | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 偵測和重新啟動失敗的執行個體                                   | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 實作清空和輪流重新啟動以進行更新                       | ![Azure][1] | ![Azure][1] | ![Azure][1] | &#x1F449; |
| 基礎結構管理                                                   | ![Azure][1] | ![Azure][1] | &#x1F449;   | &#x1F449; |
| 監視和警示管理                                             | &#x1F449;   | &#x1F449;   | &#x1F449;   | &#x1F449; |

如果您將 servlet 容器 (例如 Spring Boot) 部署為應用程式的一部分，則應該將它視為程式庫，如此一來，它始終就是您的責任。

## <a name="ensuring-on-premises-connectivity"></a>確保內部部署連線能力

如果您的應用程式需要存取您的任何內部部署服務，您必須佈建其中一個 Azure 連線能力服務。 如需詳細資訊，請參閱[選擇將內部部署網路連線到 Azure 的解決方案](/azure/architecture/reference-architectures/hybrid-networking/)。 或者，您必須重構應用程式，以使用內部部署資源所顯示的公開可用 API。

在開始任何遷移之前，您應該先完成這方面的成果。

## <a name="inventory-current-capacity-and-resource-usage"></a>清查目前容量和資源使用量

記載目前實際執行伺服器的硬體，加上平均和尖峰要求計數和資源使用量。 您將需要這項資訊，才能在服務目的地中佈建資源。

## <a name="migration-guidance"></a>移轉指導

使用下列方格，依應用程式類型和設為目標的 Azure 服務目的地尋找遷移指引。

**Java 應用程式**

使用下列資料列來尋找您的 Java 應用程式類型和資料行，以尋找將裝載您應用程式的 Azure 服務目的地。

|目的地&nbsp;→<br><br>應用程式&nbsp;類型&nbsp;↓|App<br>服務<br>Java SE|App<br>服務<br>Tomcat|App<br>服務<br>WildFly|Azure<br>Spring<br>Cloud|AKS|虛擬機器|
|---|---|---|---|---|---|---|
| Spring Boot /<br>JAR 應用程式 | [可用][5] | 已規劃        | 已規劃 | 已規劃 | 已規劃        | 已規劃 |
| Spring Cloud /<br>微服務   | N/A            | N/A            | N/A     | 已規劃 | 已規劃        | 已規劃 |
| Web 應用程式<br>在 Tomcat 上     | N/A            | [可用][2] | N/A     | N/A     | [可用][3] | 已規劃 |

**Java EE 應用程式**

使用下列資料列來尋找您在特定應用程式伺服器上執行的 Java EE 應用程式類型。 使用資料行來尋找將裝載您應用程式的 Azure 服務目的地。

|目的地&nbsp;→<br><br>應用程式伺服器&nbsp;↓|App<br>服務<br>Java SE|App<br>服務<br>Tomcat|App<br>服務<br>WildFly|Azure<br>Spring<br>Cloud|AKS|虛擬機器|
|---|---|---|---|---|---|---|
| WildFly /<br>JBoss AS | N/A | N/A | 已規劃 | N/A | 已規劃 | 已規劃        |
| WebLogic              | N/A | N/A | 已規劃 | N/A | 已規劃 | [可用][4] |
| WebSphere             | N/A | N/A | 已規劃 | N/A | 已規劃 | 已規劃        |
| JBoss EAP             | N/A | N/A | 已規劃 | N/A | N/A     | 已規劃        |

<!-- reference links, for use with tables -->
[1]: media/migration-overview/logo_azure.svg
[2]: migrate-tomcat-to-tomcat-app-service.md
[3]: migrate-tomcat-to-containers-on-azure-kubernetes-service.md
[4]: migrate-weblogic-to-virtual-machines.md
[5]: migrate-java-se-to-java-se-app-service.md