---
title: 使用 Azure Functions 撰寫無伺服器 Node.js 程式碼
description: 有關如何使用 Azure Functions 來建立和部署無伺服器程式碼的指引。
author: kraigb
manager: barbkess
ms.devlang: nodejs
ms.topic: article
ms.service: azure-nodejs
ms.date: 08/19/2019
ms.author: kraigb
ms.custom: seo-javascript-september2019, seo-javascript-october2019
ms.openlocfilehash: a985578312a2c7cb722307bf8b291eaf02905e2c
ms.sourcegitcommit: bed07b313eeab51281d1a6d4eba67a75524b2f57
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2019
ms.locfileid: "72172158"
---
# <a name="use-azure-functions-to-write-serverless-nodejs-code-on-azure"></a>使用 Azure Functions 在 Azure 上撰寫無伺服器 Node.js 程式碼

在 Azure 上，無伺服器供應項目稱為 Azure Functions。 無伺服器程式碼可讓您在網際網路上建立回應式的隨選端點，而不需擔心佈建或管理基礎結構。 無伺服器程式碼是由執行來回應各種事件的指令碼或其他程式碼所組成。 

首先，直接跳至：

- [使用 Visual Studio Code 建立第一個函式](/azure/azure-functions/functions-create-first-function-vs-code)。 本文會為您在 Visual Studio Code 內容中導入 Azure Functions，可簡化許多詳細資料。

接下來，藉由檢閱下列文章，進一步了解 Azure Functions 可執行的動作：

- [Azure Functions 簡介](/azure/azure-functions/functions-overview)，其中說明無伺服器開發的優點、成本，以及您可以用來執行無伺服器程式碼的不同觸發程序。

- [Azure Functions 觸發程序和繫結概念](/azure/azure-functions/functions-triggers-bindings)

- [Azure Functions 開發人員指南](/azure/azure-functions/functions-reference)，後面接著是 [Azure Functions JavaScript 開發人員指南](/azure/azure-functions/functions-reference-node)

- 如果您想要在無伺服器環境中撰寫「具狀態」  函式，請參閱[什麼是 Durable Functions？](/azure/azure-functions/durable/durable-functions-overview)，以及[使用 JavaScript 建立第一個耐久函式](/azure/azure-functions/durable/quickstart-js-vscode)。

從這裡，您可以享用一些其他資源，協助您進一步探索無伺服器程式碼：

- Microsoft Learn 模組：[使用 Azure 函式和 SignalR Service 在 Web 應用程式中啟用自動更新](https://docs.microsoft.com/learn/modules/automatic-update-of-a-webapp-using-azure-functions-and-signalr/)

- 了解如何使用各種觸發程序來執行無伺服器程式碼：

  - [在計時器上執行程式碼](/azure/azure-functions/functions-create-scheduled-function)
  - [在 Azure Blob 儲存體中上傳或更新檔案時執行程式碼](/azure/storage/blobs/storage-upload-process-images?tabs=nodejsv10)
  - [將訊息寫入 Azure 佇列儲存體時執行程式碼](/azure/azure-functions/functions-create-storage-queue-triggered-function)

- [使用 Azure Functions 和 Azure Cosmos DB 儲存非結構化資料](/azure/azure-functions/functions-integrate-store-unstructured-data-cosmosdb.md?tabs=javascript)。 如需其他資料庫的詳細資訊，請參閱[如何在 Node.js 程式碼中整合 Azure 資料庫](node-howto-integrate-databases.md)

- [撰寫 Azure Functions 並在本機進行測試](/azure/azure-functions/functions-develop-local)

- [在 Azure Functions 中測試程式碼的策略](/azure/azure-functions/functions-test-a-function)和[錯誤處理](/azure/azure-functions/functions-bindings-error-pages)

- [使用 Azure Active Directory 設定驗證](/azure/app-service/configure-authentication-provider-aad.md?toc=%2fazure%2fazure-functions%2ftoc.json)

- [使用 Azure Pipelines 設定持續部署](/azure/azure-functions/functions-how-to-azure-devops)

- [Node.js + Azure Functions 範例](/samples/browse/?languages=javascript%2Cnodejs&products=azure-functions)
