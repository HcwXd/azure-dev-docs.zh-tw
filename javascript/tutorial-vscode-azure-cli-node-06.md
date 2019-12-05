---
title: 對應用程式碼進行變更並重新部署至 Azure
description: 教學課程第 6 部分：進行變更並重新部署
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 9702f10795893004965631fa99dfbfab181f2292
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467167"
---
# <a name="make-changes-and-redeploy"></a>進行變更並重新部署

[上一步：串流記錄](tutorial-vscode-azure-cli-node-05.md)

在此步驟中，您會對應用程式碼進行變更、將它們認可至本機 Git 存放庫，然後藉由推送至 Azure 來重新部署您的網站。

1. 在 `myExpressApp` 資料夾中，開啟 *views/index.pug* 檔案，然後將第 5 行中的訊息變更為 `p Welcome to Azure!`。

    ![編輯 index.pug 檔案](media/azure-cli/editpugfile.png)

1. 儲存檔案。

1. 在終端或命令提示字元中，執行下列命令，將變更認可至 git：

    ```bash
    git commit -a -m "Edited message"
    ```

1. 將變更推送至我們稍早建立的 Git 遠端 (名為 Azure)：

    ```bash
    git push azure master
    ```

1. 因為 App Service 已經連線至 Git 存放庫，所以來自命令的輸出會顯示變更已自動發佈至 Azure： 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the default node.js version 6.9.5.
    remote: Selected npm version 3.10.10
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. 在瀏覽器中重新整理應用程式，以查看這些變更：

    ![已發佈變更會顯示在瀏覽器中](media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [我看到我的變更](tutorial-vscode-azure-cli-node-07.md) [我遇到問題](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
