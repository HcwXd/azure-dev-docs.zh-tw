---
title: 作業設定參數 - 適用於 Python 的 Azure SDK
description: 適用於 Python 的 Azure SDK 擲回的 C
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.custom: seo-python-october2019
ms.openlocfilehash: ca69b72789f28445c4654e635e641e2954890a38
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172366"
---
# <a name="parameters-for-operation-configuration"></a>作業設定參數

作業的方法有可在 `kwargs` 中提供的額外參數。 這稱為 operation_config。

作業組態的選項如下：

|參數名稱|類型|角色|
|----------------------|------|---------------|
| 驗證 |`bool`|是否要驗證 SSL 憑證。 預設值是 True。|
|  cert |`str`| 進行用戶端驗證的本機憑證路徑。|
|  timeout |`int`| 建立伺服器連線逾時 (以秒為單位)。|
|  allow_redirects |`bool` | 是否允許重新導向。|
|  max_redirects  |`int`| 允許的重新導向數目上限。|
|  Proxy  |`dict` |Proxy 伺服器設定。|
|  use_env_proxies |`bool` |是否從本機環境變數中讀取 Proxy 設定。|
|  重試  |`int` | 嘗試重試的次數。|
|  enable_http_logger | `bool`| 啟用偵錯模式中的 HTTP 記錄 (預設為 False)。|
