---
ms.openlocfilehash: 5b817be30b4835525ad2e25b5e3975fba1217f01
ms.sourcegitcommit: 945e92dae2fa4521eebdc049c65273ae6b5470ee
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813775"
---
安裝 Azure 擴充功能後，請登入您的 Azure 帳戶，方法為瀏覽至 **Azure** 總管，選取 [登入 Azure]  ，然後依照提示操作。 (如果您已安裝多個 Azure 擴充功能，請針對您工作所在的區域選取其中一個，例如 App Service、Functions 等)。

![透過 VS Code 登入 Azure](../media/deploy-azure/azure-sign-in.png)

登入之後，驗證 Azure 帳戶 (或 [已登入]) 的電子郵件地址是否出現在狀態列中，以及您的訂用帳戶是否出現在 **Azure** 總管：

![顯示 Azure 帳戶的 VS Code 狀態列](../media/deploy-azure/azure-account-status-bar.png)

![顯示訂用帳戶的 VS Code Azure App Service 總管](../media/deploy-azure/azure-subscription-view.png)

> [!NOTE]
> 如果您看到 **「找不到名稱為 [訂用帳戶識別碼] 的訂用帳戶**」錯誤，這可能是因為您位於 Proxy 後方，所以無法連線到 Azure API。 請使用您終端機中的Proxy 資訊來設定 `HTTP_PROXY` 和 `HTTPS_PROXY` 環境變數：
>
> ```sh
> # macOS/Linux
> export HTTPS_PROXY=https://username:password@proxy:8080
> export HTTP_PROXY=http://username:password@proxy:8080
>
> # Windows
> set HTTPS_PROXY=https://username:password@proxy:8080
> set HTTP_PROXY=http://username:password@proxy:8080
> ```
