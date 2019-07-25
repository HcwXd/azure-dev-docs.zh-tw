---
title: 適用於 Python 的 Azure SDK 作業組態
description: 適用於 Python 的 Azure SDK 擲回的 C
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 03/07/2018
ms.topic: conceptual
ms.devlang: python
ms.openlocfilehash: 9638aa4602f96e2da0155a7b3840e5be4857eb98
ms.sourcegitcommit: 2efdb9d8a8f8a2c1914bd545a8c22ae6fe0f463b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/15/2019
ms.locfileid: "68285509"
---
# <a name="operation-config"></a>作業組態 

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
|  enable_http_logger | `bool`| 啟用偵錯模式中的的 HTTP 記錄 (預設為 False)。|
