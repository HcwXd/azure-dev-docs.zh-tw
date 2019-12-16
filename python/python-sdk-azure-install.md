---
title: 安裝適用於 Python 的 Azure SDK
description: 如何使用 PIP 或 GitHub 安裝適用於 Python 的 Azure SDK。 Azure SDK 能夠以個別程式庫或完整套件的形式進行安裝。
ms.date: 10/31/2019
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: d3d162bc14f27a9b2bc3fb033dab36dcd1dfeb89
ms.sourcegitcommit: 68a4044b9fa3291c9e7e2f68ae0049328f9c01bb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2019
ms.locfileid: "74992492"
---
# <a name="install-the-azure-sdk-for-python"></a>安裝適用於 Python 的 Azure SDK

適用於 Python 的 Azure SDK 會提供 API，讓您可以從 Python 程式碼中與 Azure 互動。 您可以根據自己的需求從 SDK 安裝個別程式庫，也可以同時安裝一組完整的程式庫。

CPython 版本 2.7 和 3.5.3+ 及 PyPy 5.4+ 已針對適用於 Python 的 Azure SDK 進行測試，並且可提供支援。 開發人員也會搭配其他解譯器 (例如 IronPython 和 Jython) 來使用 SDK，但您可能會遇到隔離的問題和不相容的情況。 如果您需要 Python 解譯器，請從 [python.org/downloads](https://www.python.org/downloads) 安裝最新版本。

## <a name="install-sdk-libraries-using-pip"></a>使用 PIP 安裝 SDK 程式庫

適用於 Python 的 Azure SDK 是由數個個別的程式庫所組成，每一個都會佈建或使用特定 Azure 服務。 您可以使用 `pip install <library>` 來安裝每個程式庫。 如需每個程式庫的特定指示和文件，請參閱 [SDK 版本頁面](https://azure.github.io/azure-sdk/releases/latest/python.html)。

例如，如果您使用 Azure 儲存體，您可能會安裝 `azure-storage-file`、`azure-storage-blob` 或 `azure-storage-queue` 程式庫。 如果您要使用 Azure Cosmos DB 資料表，請安裝 `azure-cosmosdb-table`。 Azure Functions 是透過 `azure-functions` 程式庫受到支援，依此類推。 以 `azure-mgmt-` 開頭的程式庫會提供用於佈建 Azure 資源的 API 給您。

### <a name="install-specific-library-versions"></a>安裝特定的程式庫版本

如果您需要安裝特定的程式庫版本，請在命令列上指定版本：

```bash
pip install azure-storage-blob==12.0.0
```

### <a name="install-preview-packages"></a>安裝預覽套件

Microsoft 會定期發行預覽 SDK 程式庫，以支援即將推出的功能。 若要安裝最新的程式庫預覽，請在命令列上包含 `--pre` 旗標。 

```bash
# Install all preview versions of the Azure SDK for Python
pip install --pre azure

# Install the preview version for azure-storage-blob only.
pip install --pre azure-storage-blob
```

## <a name="verify-sdk-installation-details-with-pip"></a>使用 PIP 確認 SDK 安裝詳細資料

使用 `pip show <library>` 命令來確認是否已安裝程式庫。 如果已安裝程式庫，此命令會顯示版本和其他摘要資訊。 如果未安裝程式庫，此命令不會顯示任何內容。

```bash
# Check installation of the Azure SDK for Python
pip show azure

# Check installation of a specific library
pip show azure-storage-blob
```

您也可以使用 `pip freeze` 或 `pip list` 來查看目前 Python 環境中安裝的所有程式庫。

## <a name="uninstall-azure-sdk-for-python-libraries"></a>解除安裝適用於 Python 的 Azure SDK 程式庫

若要解除安裝個別的程式庫，請使用 `pip uninstall <library>`。

## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [了解如何使用 SDK](python-sdk-azure-get-started.yml)
