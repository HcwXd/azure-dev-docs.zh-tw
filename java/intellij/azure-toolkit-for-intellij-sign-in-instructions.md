---
title: Azure Toolkit for IntelliJ 的登入指示
description: 了解如何使用適用於 IntelliJ 的 Azure 工具組登入 Microsoft Azure。
documentationcenter: java
ms.date: 02/01/2018
ms.service: multiple
ms.tgt_pltfrm: multiple
ms.topic: article
ms.openlocfilehash: 345880fae8b3ef1c72833327263b76cffc96bf78
ms.sourcegitcommit: b3b7dc6332c0532f74d210b2a5cab137e38a6750
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/04/2019
ms.locfileid: "74812485"
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Azure Toolkit for IntelliJ 的登入指示

一旦[安裝](https://www.jetbrains.com/help/idea/managing-plugins.html)，[Azure Toolkit for IntelliJ](https://plugins.jetbrains.com/plugin/8053) 會提供兩種登入您的 Azure 帳戶的方法︰

  - [使用裝置登入來登入您的 Azure 帳戶](#sign-in-to-your-azure-account-by-device-login)
  - [使用服務主體來登入您的 Azure 帳戶](#sign-in-to-your-azure-account-by-service-principal)

此外也提供[**登出**](#sign-out-of-your-azure-account)方法。

[!INCLUDE [azure-toolkit-for-intellij-basic-prerequisites](../includes/azure-toolkit-for-intellij-basic-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-by-device-login"></a>使用裝置登入來登入您的 Azure 帳戶

若要使用裝置登入來登入 Azure，請執行下列作業：

1. 使用 IntelliJ IDEA 開啟您的專案。

2. 開啟 **Azure 總管**資訊看板，然後按一下頂端列中的 [Azure 登入]  圖示 (或從 IDEA 功能表的**工具/Azure/Azure 登入**點選)。

   ![IntelliJ 的 [Azure 登入] 命令][I01]

3. 在 [Azure 登入]  視窗中，選取 [裝置登入]  ，然後按一下 [登入]  。

   ![選取了 [裝置登入] 的 [Azure 登入] 視窗][I02]

4. 按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。

   ![[Azure 登入] 對話方塊視窗][I03]

5. 在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。

   ![裝置登入瀏覽器][I04]

6. 在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。

   ![[選取訂用帳戶] 對話方塊][I05]

## <a name="sign-in-to-your-azure-account-by-service-principal"></a>使用服務主體來登入您的 Azure 帳戶

本節會逐步引導您建立認證檔案，並在檔案中包含您的服務主體資料。 在完成此程序後，IntelliJ 會使用認證檔案，讓您在開啟專案時自動登入 Azure。

1. 使用 IntelliJ IDEA 開啟您的專案。

1. 開啟 **Azure 總管**資訊看板，然後按一下頂端列中的 [Azure 登入]  圖示 (或從 IDEA 功能表的**工具/Azure/Azure 登入**點選)。
   ![IntelliJ 的 [Azure 登入] 命令][A01]

1. 在 [Azure 登入]  視窗中，選取 [服務主體]  ，然後按一下 [新增]  。

   ![選取了 [服務主體] 的 [Azure 登入] 視窗][A02]

1. 按一下 [Azure 裝置登入]  對話方塊中的 [複製並開啟]  。

   ![[Azure 登入] 對話方塊視窗][A03]

1. 在瀏覽器中，貼上您的裝置程式碼 (您在上一個步驟中按一下 [複製並開啟]  時所複製)，然後按 [下一步]  。

   ![裝置登入瀏覽器][A04]

1. 在 [建立驗證檔案]  視窗中，選取您想要使用的訂用帳戶，選擇您的目的地目錄，然後按一下 [啟動]  。

   ![[建立驗證檔案] 視窗][A05]

1. 在 [服務主體建立狀態]  對話方塊中，於系統成功建立檔案之後按一下 [確定]  。

   ![[服務主體建立狀態] 對話方塊][A06]

1. 在 [Azure 登入]  視窗中，按一下 [登入]  。 

   ![[Azure 登入] 對話方塊][A07]

1. 在 [選取訂用帳戶]  對話方塊中，選取您要使用的訂用帳戶，然後按一下 [確定]  。

   ![[選取訂用帳戶] 對話方塊][A08]

> 建立服務主體驗證檔案之後，您可以從步驟 8 開始，選擇您的驗證檔案並登入。

## <a name="sign-out-of-your-azure-account"></a>登出您的 Azure 帳戶

在前述步驟中設定好帳戶後，每當您啟動 IntelliJ IDEA 時，系統就會將您自動登入。 不過，如果您想要登出 Azure 帳戶，請執行下列作業。

* 在 IntelliJ IDEA 中，開啟 [Azure 總管] 資訊看板，按一下 [Azure 登出]  圖示並確認 (或從 IDEA 功能表的**工具/Azure/Azure 登出**點選)。

   ![IntelliJ 的 [Azure 登出] 命令][L01]

## <a name="next-steps"></a>後續步驟

[!INCLUDE [azure-toolkit-for-intellij-additional-resources](../includes/azure-toolkit-for-intellij-additional-resources.md)]

<!-- URL List -->

<!-- IMG List -->

[I01]: media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: media/azure-toolkit-for-intellij-sign-in-instructions/I04.png
[I05]: media/azure-toolkit-for-intellij-sign-in-instructions/I05.png

[A01]: media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: media/azure-toolkit-for-intellij-sign-in-instructions/A08.png
[A09]: media/azure-toolkit-for-intellij-sign-in-instructions/A09.png

[L01]: media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
