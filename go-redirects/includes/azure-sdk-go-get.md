---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: a51c8667c74a2611ae8769aa42fd1a94f9253bc8
ms.sourcegitcommit: 9cd460ee16b637e701aa30078932878c0d0a7945
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2019
ms.locfileid: "70180801"
---
[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) 與 Go 1.8 版和更新版本相容。 若為使用 [Azure Stack 設定檔](/azure/azure-stack/user/azure-stack-version-profiles-go)的環境，Go 1.9 版是最低需求。
如果您需要安裝 Go，請遵循 [Go 安裝指示](https://golang.org/doc/install)。

您可以透過 `go get` 下載 Azure SDK for Go 與其相依性。

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> 務必將 URL 中的 `Azure` 變成大寫。 使用 SDK 時，若未這麼做可能會造成與大小寫相關的匯入問題。 您也需要將您匯入陳述式中的 `Azure` 變成大寫。
