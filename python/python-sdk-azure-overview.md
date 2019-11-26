---
title: 適用於 Python 的 Azure SDK
description: 概述適用於 Python 的 Azure SDK 有何特性和功能，可協助開發人員在使用 Azure 服務時更具生產力。
author: kraigb
ms.author: kraigb
manager: barbkess
ms.service: multiple
ms.date: 10/30/2019
ms.topic: conceptual
ms.devlang: python
ms.openlocfilehash: dda3044bcf80ff45d4a39c9186c6ad44a153218e
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118011"
---
# <a name="azure-sdk-for-python"></a>適用於 Python 的 Azure SDK

適用於 Python 的 Azure SDK 可簡化從 Python 應用程式程式碼中使用和管理 Azure 資源的工作。 SDK 支援 Python 2.7 和 Python 3.5.3 或更新版本。

您可以藉由使用 `pip install <library>` 安裝 SDK 的任何個別元件程式庫來安裝 SDK。 您可以在[適用於 Python 的 Azure SDK 套件索引](https://github.com/Azure/azure-sdk-for-python/blob/master/packages.md)上看到程式庫清單

如需安裝程式庫並將其匯入專案的詳細資訊，請參閱[安裝 SDK](python-sdk-azure-install.md)。 然後檢閱[開始使用 SDK](python-sdk-azure-get-started.yml)，針對您自己的 Azure 訂用帳戶設定驗證並執行範例程式碼。

> [!TIP]
> 如需有關 SDK 變更的詳細資訊，請參閱 [SDK 版本資訊](https://azure.github.io/azure-sdk/)。

## <a name="connect-and-use-azure-services"></a>連線與使用 Azure 服務

SDK 中有許多「用戶端程式庫」  可協助您連線到現有的 Azure 資源，並在您應用程式中使用，例如上傳檔案、存取資料表資料，或使用各種 Azure 認知服務。 使用 SDK 時，您可以使用熟悉的 Python 程式設計架構來處理這些資源，而不是使用服務的通用 REST API。

例如，假設您想要將 Blob 上傳至先前佈建的 Azure 儲存體帳戶。 第一個步驟是安裝適當的程式庫：

```bash
pip install azure-storage-blob
```

接下來，在您的程式碼中匯入此程式庫：

```python
from azure.storage.blob import BlobClient
```

最後，使用程式庫的 API 來連線並上傳資料。 在此範例中，您的儲存體帳戶中已佈建連接字串和容器名稱。 Blob 名稱是您指派給已上傳資料的名稱：

```python
blob = BlobClient.from_connection_string("my_connection_string", container="mycontainer", blob="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

如需使用每個特定程式庫的詳細資訊，請在 [GitHub 存放庫](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的程式庫專案資料夾中參閱 README.md  或 README.rst  檔案。 另請參閱可用的 [Azure 範例](https://docs.microsoft.com/samples/browse/?languages=python)。

您也可以在[參考文件](/python/api?view=azure-python)中找到其他程式碼片段

### <a name="the-azure-core-library"></a>Azure 核心程式庫

我們目前正在更新 Python 用戶端程式庫來共用核心功能，例如重試、記錄、傳輸通訊協定和驗證通訊協定等。此共用功能包含在 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 程式庫中。 如需有關程式庫及其指導方針的詳細資訊，請參閱 [Python 指導方針：簡介](https://azure.github.io/azure-sdk/python_introduction.html)。

目前與核心程式庫搭配使用的程式庫如下所示：

- `azure-storage-blob`
- `azure-storage-queue`
- `azure-keyvault-keys`
- `azure-keyvault-secrets`

## <a name="manage-azure-resources"></a>管理 Azure 資源

適用於 Python 的 Azure SDK 也包含許多程式庫，其本身可協助您建立、佈建和管理 Azure 資源。 我們將這些稱為「管理程式庫」  。 每個管理程式庫的名稱都是 `azure-mgmt-<service name>`。 透過管理程式庫，您可以撰寫 Python 程式碼來完成使用 [Azure 入口網站](https://portal.azure.com) 或 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 達成的相同工作。

例如，假設您要建立 SQL Server 執行個體。 首先，安裝適當的管理程式庫：

```bash
pip install azure-mgmt-sql
```

在您的 Python 程式碼中，匯入程式庫：

```python
from azure.mgmt.sql import SqlManagementClient

```

接下來，使用您的認證和 Azure 訂用帳戶識別碼來建立管理用戶端物件：

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

最後，使用該用戶端物件來建立資源，並使用適當的資源群組名稱、伺服器名稱、位置和系統管理員認證：

```python
server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

如同用戶端程式庫，您可以在 [GitHub 存放庫](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的程式庫專案資料夾中取得 README.md  或 README.rst  檔案，以了解每個管理程式庫的使用詳細資訊。

您也可以在[參考文件](/python/api?view=azure-python)中找到其他程式碼片段。 

## <a name="get-help-and-give-feedback"></a>獲得協助及提供意見

- 請瀏覽[適用於 Python 的 Azure SDK 文件](https://aka.ms/python-docs)
- 您可以在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) 的社群中張貼問題
- 在 [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) 上提出 SDK 的相關問題
- 在 [@AzureSDK](https://twitter.com/AzureSdk/) 上推文
