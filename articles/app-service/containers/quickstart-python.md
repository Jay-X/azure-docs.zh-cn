---
title: 在 Linux 上创建 Python 应用 - Azure 应用服务 | Microsoft Docs
description: 数分钟内在 Linux 上的 Azure 应用服务中部署第一个 Python hello world 应用。
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 01/23/2019
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: be78c91a4fb5c1e79e7b58620f65c9f17bfb4bae
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55226479"
---
# <a name="create-a-python-app-in-azure-app-service-on-linux-preview"></a>在 Linux 上的 Azure 应用服务（预览）中创建 Python 应用

[Linux 应用服务](app-service-linux-intro.md)使用 Linux 操作系统，提供高度可缩放的自修补 Web 托管服务。 本快速入门展示了如何使用 [Azure CLI](/cli/azure/install-azure-cli) 在 Linux 应用服务中，在内置 Python 映像（预览）之上部署 Python 应用。

可以使用 Mac、Windows 或 Linux 计算机执行本文中的步骤。

![在 Azure 中运行应用的示例](media/quickstart-python/hello-world-in-browser.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

完成本快速入门教程需要：

* <a href="https://www.python.org/downloads/" target="_blank">安装 Python 3.7</a>
* <a href="https://git-scm.com/" target="_blank">安装 Git</a>

## <a name="download-the-sample"></a>下载示例

在终端窗口中运行以下命令，将示例应用程序克隆到本地计算机，并导航到包含示例代码的目录。

```bash
git clone https://github.com/Azure-Samples/python-docs-hello-world
cd python-docs-hello-world
```

存储库包含一个 *application.py*，后者会告知应用服务：存储库包含 Flask 应用。 有关详细信息，请参阅[容器启动过程和自定义项](how-to-configure-python.md)。

## <a name="run-the-app-locally"></a>在本地运行应用

在本地运行应用程序，以便你能了解将它部署到 Azure 时它的外观应该是什么样的。 打开终端窗口，使用以下命令安装所需的依赖项，并启动内置开发服务器。 

```bash
# In Bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
FLASK_APP=application.py flask run

# In PowerShell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

打开 Web 浏览器并导航到 `http://localhost:5000/` 处的示例应用。

此时会看到来自示例应用的 Hello World! 消息显示在页面中。

![本地运行应用的示例](media/quickstart-python/hello-world-in-browser.png)

在终端窗口中，按 **Ctrl+C** 退出 Web 服务器。

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>创建 Web 应用

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-python-linux-no-h.md)]

浏览到该站点查看使用内置映像新建的应用。 将 _&lt;app name>_ 替换为你的应用名称。

```bash
http://<app_name>.azurewebsites.net
```

新应用应该如下所示：

![空应用页](media/quickstart-php/app-service-web-service-created.png)

[!INCLUDE [Push to Azure](../../../includes/app-service-web-git-push-to-azure.md)] 

```bash
Counting objects: 42, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (39/39), done.
Writing objects: 100% (42/42), 9.43 KiB | 0 bytes/s, done.
Total 42 (delta 15), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'c40efbb40e'.
remote: Generating deployment script.
remote: Generating deployment script for python Web Site
.
.
.
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://user2234@cephalin-python.scm.azurewebsites.net/cephalin-python.git
 * [new branch]      master -> master
 ```

## <a name="browse-to-the-app"></a>浏览到应用

使用 Web 浏览器浏览到已部署的应用程序。

```bash
http://<app_name>.azurewebsites.net
```

Python 示例代码在包含内置映像的 Linux 上的应用服务中运行。

![在 Azure 中运行应用的示例](media/quickstart-python/hello-world-in-browser.png)

祝贺你！ 现已将第一个 Python 应用部署到 Linux 应用服务。

## <a name="update-locally-and-redeploy-the-code"></a>在本地更新并重新部署代码

在本地存储库中，打开 `application.py` 文件，并对最后一行中的文本略加更改：

```python
return "Hello Azure!"
```

提交在 Git 中所做的更改，然后将代码更改推送到 Azure。

```bash
git commit -am "updated output"
git push azure master
```

完成部署后，切换回**浏览到应用**步骤中打开的浏览器窗口，然后刷新页面。

![已更新的在 Azure 中运行应用的示例](media/quickstart-python/hello-azure-in-browser.png)

## <a name="manage-your-new-azure-app"></a>管理新的 Azure 应用

转到 <a href="https://portal.azure.com" target="_blank">Azure 门户</a>管理已创建的应用。

在左侧菜单中单击“应用程序服务”，然后单击 Azure 应用的名称。

![在门户中导航到 Azure 应用](./media/quickstart-python/app-service-list.png)

这里我们可以看到应用的“概述”页。 并可以执行基本的管理任务，例如浏览、停止、启动、重新启动和删除。

![Azure 门户中的应用服务页](media/quickstart-python/app-service-detail.png)

左侧菜单提供了用于配置应用的不同页面。 

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>后续步骤

Linux 上的应用服务中内置的 Python 映像目前处于预览状态，你可以自定义用于启动应用的命令。 还可以改用自定义容器创建生产 Python 应用。

> [!div class="nextstepaction"]
> [将 Python 与 PostgreSQL 配合使用](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [配置自定义启动命令](how-to-configure-python.md#customize-startup-command)

> [!div class="nextstepaction"]
> [故障排除](how-to-configure-python.md#troubleshooting)

> [!div class="nextstepaction"]
> [使用自定义映像](tutorial-custom-docker-image.md)
