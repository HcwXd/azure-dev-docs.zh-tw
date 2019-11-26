---
title: 移至 Java 11 的原因
description: 一份摘要層級文件，旨在供決策者權衡從 Java 8 移至 Java 11 的優點。
author: dsgrieve
manager: maverberg
tags: java
ms.service: azure
ms.devlang: java
ms.topic: article
ms.date: 11/19/2019
ms.author: dagrieve
ms.openlocfilehash: ed9b4d7e98357486367f7e7eaacac64ff05a0ff8
ms.sourcegitcommit: 90068e30def5dfcb4289d8530ea5914728182a15
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/20/2019
ms.locfileid: "74250745"
---
# <a name="reasons-to-move-to-java-11"></a>移至 Java 11 的原因

問題並非您*是否*應移至 Java 11，而是移轉的*時機*。 在接下來的幾年內，將不再支援 Java 8，使用者必須移至 Java 11。 我們認為移至 Java 11 有其好處，並鼓勵小組盡快進行移轉。

自 Java 8 以來，不僅已加入了新功能，也做了許多強化。 API 有顯著的新增和修改，此外也有增強功能可改善啟動、效能和記憶體使用量。

## <a name="transitioning-to-java-11"></a>轉換至 Java 11

轉換至 Java 11 的作業可透過逐步方式來完成。 程式碼*不*需要使用 Java 模組即可在 Java 11 上執行。 Java 11 可用來執行以 JDK 8 開發和建置的程式碼。
但這會有一些潛在的問題，主要是關於已被取代的 API、類別載入器和反映。

Microsoft Java 平台部門即將推出從 Java 8 轉換至 Java 11 的完整指南。 同時，還有許多從 Java 8 轉換至 Java 9 的指南，可協助您展開作業。 例如，[Java Platform Standard Edition Oracle JDK 9 移轉指南](https://docs.oracle.com/javase/9/migrate/toc.htm)和[模組系統的狀態：相容性和移轉](http://openjdk.java.net/projects/jigsaw/spec/sotms/#compatibility--migration)。

## <a name="high-level-changes-between-java-8-and-11"></a>Java 8 與 11 之間的高層級變更

本節不會列舉 Java 9 \[[1](#ref1)\]、10 \[[2](#ref2)\] 和 11 \[[3](#ref3)\] 版中所做的所有變更。 對效能、診斷和生產力有所影響的變更會摘要列出。

### <a name="modules-4ref4"></a>模組 \[[4](#ref4)\]

模組可解決在執行於*類別路徑*上的大規模應用程式中難以管理設定和封裝的問題。 *模組*是一種自我描述的集合，由 Java 類別和介面以及相關資源組成。

模組可讓您自訂僅包含應用程式所需元件的執行階段設定。 這項自訂會產生較少的資源使用量，並允許應用程式使用 [jlink](https://docs.oracle.com/en/java/javase/11/tools/jlink.html) 以靜態方式連結到自訂執行階段，以進行部署。 這種較少的資源使用量在微服務架構中特別有用。

就內部而言，JVM 可透過讓類別載入更有效率的方式利用模組。 其結果是執行階段更小、更精簡，且能夠更快啟動。 JVM 用來改善應用程式效能的最佳化技術會更有效率，因為模組會編碼類別所需的元件。

對程式設計人員而言，模組可藉由要求明確宣告模組所匯出的套件及其所需的元件，以及限制反射性存取，來協助強制執行強式封裝。
此種封裝層級可讓應用程式更安全且更容易維護。

應用程式可以繼續使用*類別路徑*，且不需要轉換成模組即可在 Java 11 上執行。

### <a name="profiling-and-diagnostics"></a>分析和診斷

#### <a name="java-flight-recorder-5ref5"></a>Java Flight Recorder \[[5](#ref5)\]

Java Flight Recorder (JFR) 會從執行中的 Java 應用程式收集診斷和分析資料。 JFR 對執行中的 Java 應用程式不會有太大的影響。 然後，可使用 Java Mission Control (JMC) 和其他工具來分析收集到的資料。 JFR 和 JMC 是 Java 8 中的商用功能，但兩者在 Java 11 中都是開放原始碼。

#### <a name="java-mission-control-6ref6"></a>Java Mission Control \[[6](#ref6)\]

Java Mission Control (JMC) 可提供 Java Flight Recorder (JFR) 所收集之資料的圖形化顯示，並且是 Java 中的開放原始碼
11. 除了執行中應用程式的一般資訊以外，JMC 還可讓使用者向下切入資料。 JFR 和 JMC 可以用來診斷執行階段問題，例如記憶體流失、GC 額外負荷、熱方法、執行緒瓶頸和封鎖 I/O 等。

#### <a name="unified-logging-7ref7"></a>整合記錄 \[[7](#ref7)\]

Java 11 具有 JVM 的所有元件通用的記錄系統。
此整合記錄系統可讓使用者定義要記錄的元件，以及要記錄到何種層級。 對 JVM 當機執行根本原因分析，以及診斷生產環境中的效能問題時，這種精細的記錄將可發揮功效。

#### <a name="low-overhead-heap-profiling-8ref8"></a>低額外負荷堆積分析 \[[8](#ref8)\]

Java 虛擬機器工具介面 (JVMTI) 已加入新的 API，用以取樣 Java 堆積配置。 取樣的額外負荷很低，且可以持續啟用。 雖然可以使用 Java Flight Recorder (JFR) 來監視堆積配置，但是 JFR 中的取樣方法僅適用於配置。 JFR 實作也有可能遺漏配置。 相對地，Java 11 中的堆積取樣可同時提供有效和失效物件的相關資訊。

應用程式效能監控 (APM) 廠商已開始利用這項新功能，而 Java 平台小組正在研究是否可將其與 Azure 效能監控工具搭配使用。

#### <a name="stackwalker-9ref9"></a>StackWalker \[[9](#ref9)\]

取得目前執行緒的堆疊快照集，通常是在記錄時進行的。 問題在於要記錄多少堆疊追蹤，以及究竟是否要記錄堆疊追蹤。 例如，您可能只想要針對方法中的特定例外狀況查看堆疊追蹤。 StackWalker 類別 (新增於 Java 9 中) 可提供堆疊的快照集，並提供適當方法讓程式設計人員能夠精細控制使用堆疊追蹤的方式。

### <a name="garbage-collection-10ref10"></a>記憶體回收 \[[10](#ref10)\]

以下是 Java 11 中可用的記憶體回收行程：序列、平行、Garbage-First 和 Epsilon。 Java 11 中的預設記憶體回收行程是 Garbage-First 記憶體回收行程 (G1GC)。

為求完整性，這裡會提及其他三個回收行程。 Z 記憶體回收行程 (ZGC) 是一種並行、低延遲的回收行程，會嘗試將暫停時間保持在 10 毫秒以下。 ZGC 是 Java 11 中提供的實驗性功能。 Shenandoah 回收行程是一個低暫停的回收行程，可在 Java 程式執行的同時執行更多記憶體回收，藉以減少 GC 暫停時間。
Shenandoah 是 Java 12 中的實驗性功能，但可向下移植到 Java 11。 Concurrent Mark and Sweep (CMS) 回收行程仍可供使用，但自 Java 9 起已被取代。

JVM 在一般使用案例中會將 GC 設為預設值。 這些預設值和其他 GC 設定常需要根據應用程式的需求進行調整，以取得最佳的輸送量或延遲。
要適當地調整 GC 必須具備深厚的 GC 知識，即 [Microsoft Java 工程群組](mailto:javaplatformgroup@microsoft.com)提供的專業知識。

#### <a name="g1gc"></a>G1GC

Java 11 中的預設記憶體回收行程是 G1 記憶體回收行程 (G1GC)。 G1GC 的目的是要在延遲與輸送量之間取得平衡。 G1 記憶體回收行程會嘗試以高機率達成暫停時間目標，以達到高輸送量。 G1GC 的設計宗旨是要避免完整回收，但在並行回收無法快速回收記憶體時，就會執行完整 GC。 完整 GC 會使用與早期和混合回收相同數目的平行背景工作執行緒。

#### <a name="parallel-gc"></a>平行 GC

平行回收行程是 Java 8 中的預設回收行程。 平行 GC 是使用多個執行緒來加速記憶體回收的輸送量回收行程。

#### <a name="epsilon-11ref11"></a>Epsilon \[[11](#ref11)\]

Epsilon 記憶體回收行程會處理配置，但不會回收任何記憶體。 當堆積耗盡時，JVM 將會關閉。
Epsilon 適用於短期的服務，和已知不會產生垃圾的應用程式。

#### <a name="improvements-for-docker-containers-12ref12"></a>Docker 容器的改良 \[[12](#ref12)\]

在 Java 10 之前，JVM 無法辨識對容器設定的記憶體和 CPU 條件約束。 例如，在 Java 8 中，JVM 會將堆積大小上限預設為基礎主機實體記憶體的 ¼。 從 Java 10 開始，JVM 使用容器控制項群組 (cgroups) 所設定的條件約束，來設定記憶體和 CPU 限制 (請參閱下列附註)。
例如，預設的堆積大小上限為容器記憶體限制的 ¼ (以 -m2G 為例，即為 500MB)。

此外也新增了 JVM 選項，讓 Docker 容器使用者可精細控制將用於 Java 堆積的系統記憶體數量。

這項支援預設為啟用，且僅適用於以 Linux 為基礎的平台。

> [!NOTE]
> 大部分的 cgroup 可行工作都可向下移植到 jdk8u191 之後的 Java 8。 進一步的改良功能不一定可向下移植到 8。

#### <a name="multi-release-jar-files-13ref13"></a>多版本 jar 檔案 \[[13](#ref13)\]

在 Java 11 中，您所建立的 jar 檔案可以包含多個 Java 版本特定的類別檔案版本。 多版本 jar 檔案可讓程式庫開發人員無須交付多個 jar 檔案版本即可支援多個 Java 版本。 針對這些程式庫的取用者，多版本 jar 檔案可解決特定 jar 檔案必須與特定執行階段目標相符的問題。

## <a name="miscellaneous-performance-improvements"></a>其他效能改進

下列 JVM 變更對效能有直接影響。

-   **JEP 197：分段程式碼快取** \[[14](#ref14)\] - 將程式碼快取分成不同的區段。 這種分割可讓您更有效地控制 JVM 記憶體使用量、縮短編譯方法的掃描時間、大幅減少程式碼快取的片段，並提升效能。

-   **JEP 254：壓縮字串** \[[15](#ref15)\] - 將字串的內部表示法從每個字元兩個位元組變更為每個字元一或兩個位元組，視字元編碼而定。 由於大部分的字串都包含 ISO-8859-1/Latin-1 字元，因此這項變更可將儲存字串所需的空間量有效減半。

-   **JEP 310：應用程式類別資料共用** \[[16](#ref16)\] -「類別資料共用」可讓封存的類別在執行階段進行記憶體對應，藉以縮短啟動時間。 「應用程式類別資料共用」可讓應用程式類別放在 CD 封存中，藉以擴充類別資料共用。 當多個 JVM 共用相同的封存檔案時，記憶體將會儲存，而整體系統回應時間也會改善。

-   **JEP 312：執行緒本機交握** \[[17](#ref17)\] - 可讓您在執行緒上執行回呼，而不需要執行全域 VM 安全點，這樣可藉由減少全域安全點的數目，協助 VM 達到較低的延遲。

-   **編譯器執行緒的消極式配置** \[[18](#ref18)\] - 在分層式編譯模式中，VM 會啟動大量的編譯器執行緒。
    在具有許多 CPU 的系統上，此模式是預設值。 無論可用的記憶體或編譯要求的數目為何，都會建立這些執行緒。 執行緒即使在閒置時 (絕大部分的時間都是如此) 也會耗用記憶體，這會導致資源的使用效率不佳。 為了解決此問題，實作方式已變更為在啟動期間僅啟動每個類型的一個編譯器執行緒。 啟動額外的執行緒，並關閉未使用的執行緒，會以動態方式處理。 

對核心程式庫所做的下列變更，會對新的或修改過的程式碼產生效能影響。

-   **JEP 193：變數控制代碼** \[[19](#ref19)\] - 定義標準方法，以叫用物件欄位和陣列元素的各項 java.util.concurrent.atomic 和 sun.misc.Unsafe 作業的對等項目，這是一組可用來精細控制記憶體排序的標準隔離作業，以及可確保參考的物件仍可供穩定存取的標準可連線性隔離作業。

-   **JEP 269：適用於集合的便利性 Factory 方法** \[[20](#ref20)\] - 定義程式庫 API，以方便建立具有少量元素的集合和對應執行個體。 集合介面上的靜態 Factory 方法可建立精簡、無法修改的集合執行個體。 這些執行個體原本就更有效率。 API 會建立以簡潔的方式表示、且沒有包裝函式類別的集合。

-   **JEP 285：運轉等候提示** \[[21](#ref21)\] - 提供 API，讓 Java 能夠提示執行階段系統它處於運轉迴圈中。 某些硬體平台可受益於「執行緒處於忙碌等候狀態」這類的軟體指示。

-   **JEP 321：HTTP 用戶端 (標準)** \[[22](#ref22)\] - 提供實作 HTTP/2 和 WebSocket 的新 HTTP 用戶端 API，且可以取代舊版 HttpURLConnection API。

## <a name="references"></a>參考

<a id="ref1">\[1\]</a> Oracle Corporation 的 \"Java 開發套件 9 版本資訊\" (線上)。 提供於： https://www.oracle.com/technetwork/java/javase/9u-relnotes-3704429.html 。
(2019 年 11 月 13 日存取)。

<a id="ref2">\[2\]</a> Oracle Corporation 的 \"Java 開發套件 10 版本資訊\" (線上)。 提供於： https://www.oracle.com/technetwork/java/javase/10u-relnotes-4108739.html 。
(2019 年 11 月 13 日存取)。

<a id="ref3">\[3\]</a> Oracle Corporation 的 \"Java 開發套件 11 版本資訊\" (線上)。 提供於： https://www.oracle.com/technetwork/java/javase/11u-relnotes-5093844.html 。
(2019 年 11 月 13 日存取)。

<a id="ref4">\[4\]</a> Oracle Corporation 的 \"Project Jigsaw\"，9 月 22 日 
2017. (線上)。 提供於： http://openjdk.java.net/projects/jigsaw/ 。
(2019 年 11 月 13 日存取)。

<a id="ref5">\[5\]</a> Oracle Corporation 的 \"JEP 328：Flight Recorder\"，2018 年 9 月 9 日。 (線上)。 提供於： http://openjdk.java.net/jeps/328 。 (2019 年 11 月 13 日存取)。

<a id="ref6">\[6\]</a> Oracle Corporation 的 \"Mission Control\"，2019 年 4 月 25 日。 (線上)。 提供於： https://wiki.openjdk.java.net/display/jmc/Main 。 (2019 年 11 月 13 日存取)。

<a id="ref7">\[7\]</a> Oracle Corporation 的 \"JEP 158：整合 JVM 記錄\"，2019 年 2 月 14 日。 (線上)。 提供於： http://openjdk.java.net/jeps/158 。
(2019 年 11 月 13 日存取)。

<a id="ref8">\[8\]</a> Oracle Corporation 的 \"JEP 331：低額外負荷堆積分析\"，2018 年 9 月 5 日。 (線上)。 提供於： http://openjdk.java.net/jeps/331 。 (2019 年 11 月 13 日存取)。

<a id="ref9">\[9\]</a> Oracle Corporation 的 \"JEP 259：Stack-Walking API\"，2017 年 7 月 18 日。 (線上)。
提供於： http://openjdk.java.net/jeps/259 。 (2019 年 11 月 13 日存取)。

<a id="ref10">\[10\]</a> Oracle Corporation 的 \"JEP 248：將 G1 設為預設的記憶體回收行程\"，2017 年 9 月 12 日。 (線上)。 提供於： http://openjdk.java.net/jeps/248 。 (2019 年 11 月 13 日存取)。

<a id="ref11">\[11\]</a> Oracle Corporation 的 \"JEP 318：Epsilon：無作業記憶體回收行程\"，2018 年 9 月 24 日。
(線上)。 提供於： http://openjdk.java.net/jeps/318 。 (2019 年 11 月 13 日存取)。

<a id="ref12">\[12\]</a> Oracle Corporation 的 \"JDK-8146115：改善 Docker 容器偵測和資源設定使用量\"，2019 年 9 月 16 日。
(線上)。 提供於： https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=JDK-8146115 。
(2019 年 11 月 13 日存取)。

<a id="ref13">\[13\]</a> Oracle Corporation 的 \"JEP 238：多版本 jar 檔案\"，2017 年 6 月 22 日。 (線上)。 提供於： http://openjdk.java.net/jeps/238 。 (2019 年 11 月 13 日存取)。

<a id="ref14">\[14\]</a> Oracle Corporation 的 \"JEP 197：分段程式碼快取\"，2017 年 4 月 28 日。 (線上)。
提供於： http://openjdk.java.net/jeps/197 。 (2019 年 11 月 13 日存取)。

<a id="ref15">\[15\]</a> Oracle Corporation 的 \"JEP 254：精簡字串\"，2019 年 5 月 18 日。 (線上)。 提供於： http://openjdk.java.net/jeps/254 。
(2019 年 11 月 13 日存取)。

<a id="ref16">\[16\]</a> Oracle Corporation 的 \"JEP 310：應用程式類別資料共用\"，2018 年 8 月 17 日。 (線上)。 提供於： https://openjdk.java.net/jeps/310 。 (2019 年 11 月 13 日存取)。

<a id="ref17">\[17\]</a> Oracle Corporation 的 \"JEP 312：執行緒本機交握\"，2019 年 8 月 21 日。
(線上)。 提供於： https://openjdk.java.net/jeps/312 。 (2019 年 11 月 13 日存取)。

<a id="ref18">\[18\]</a> Oracle Corporation 的 \"JDK-8198756：編譯器執行緒的消極式配置\"，2018 年 10 月 29 日。 (線上)。 提供於： https://bugs.java.com/bugdatabase/view\_bug.do?bug\_id=8198756 。
(2019 年 11 月 13 日存取)。

<a id="ref19">\[19\]</a> Oracle Corporation 的 \"JEP 193：變數控制代碼\"，2017 年 8 月 17 日。 (線上)。 提供於： https://openjdk.java.net/jeps/193 。 (2019 年 11 月 13 日存取)。

<a id="ref20">\[20\]</a> Oracle Corporation 的 \"JEP 269：適用於集合的便利性 Factory 方法\"，2017 年 6 月 26 日。 (線上)。 提供於： https://openjdk.java.net/jeps/269 。
(2019 年 11 月 13 日存取)。

<a id="ref21">\[21\]</a> Oracle Corporation 的 \"JEP 285：運轉等候提示\"，2017 年 8 月 20 日。 (線上)。 提供於： https://openjdk.java.net/jeps/285 。 (2019 年 11 月 13 日存取)。

<a id="ref22">\[22\]</a> Oracle Corporation 的 \"JEP 321：HTTP 用戶端 (標準)\"，2018 年 9 月 27 日。 (線上)。
提供於： https://openjdk.java.net/jeps/321 。 (2019 年 11 月 13 日存取)。
