---
title: 在 Azure Database for MariaDB 中创建和管理 MariaDB 防火墙规则
description: 使用 Azure 门户创建和管理 Azure Database for MariaDB 防火墙规则
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a43c47a1ac143014a7b36f64d72d20bf73c05c92
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950278"
---
# <a name="create-and-manage-azure-database-for-mariadb-firewall-rules-by-using-the-azure-portal"></a>使用 Azure 门户创建和管理 Azure Database for MariaDB 防火墙规则
使用服务器级防火墙规则，管理员可以从指定的 IP 地址或某个范围的 IP 地址访问 Azure Database for MariaDB 服务器。 

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>在 Azure 门户中创建服务器级防火墙规则

1. 在 MariaDB 服务器页上的“设置”标题下，单击“连接安全性”，以打开 Azure Database for MariaDB 的“连接安全性”页。

   ![Azure 门户 - 单击“连接安全性”](./media/howto-manage-firewall-portal/1-connection-security.png)

2. 在工具栏上单击“添加我的 IP”。 该操作会自动创建一条防火墙规则，其中包含计算机的公共 IP 地址（由 Azure 系统标识）。

   ![Azure 门户 - 单击“添加我的 IP”](./media/howto-manage-firewall-portal/2-add-my-ip.png)

3. 验证 IP 地址，并保存配置。 在某些情况下，Azure 门户识别出的 IP 地址与访问 Internet 和 Azure 服务器时所使用的 IP 地址不同。 因此，可能需要更改起始 IP 和结束 IP，以使规则正常工作。

   使用搜索引擎或其他联机工具来查看自己的 IP 地址。 例如，搜索“我的 IP 地址是多少”。

4. 添加其他地址范围。 在 Azure Database for MariaDB 防火墙规则中，可以指定单个 IP 地址，也可以指定某个范围的地址。 如果希望将规则限制为单个 IP 地址，请在“起始 IP”和“结束 IP”字段中输入相同的地址。 打开防火墙后，管理员、用户和应用程序可以访问 MariaDB 服务器上他们拥有有效凭据的任何数据库。

   ![Azure 门户 - 防火墙规则 ](./media/howto-manage-firewall-portal/4-specify-addresses.png)

5. 在工具栏上单击“保存”以保存此服务器级防火墙规则。 等待出现有关防火墙规则更新已成功的确认消息。

   ![Azure 门户 - 单击“保存”](./media/howto-manage-firewall-portal/5-save-firewall-rule.png)

## <a name="connecting-from-azure"></a>从 Azure 连接
若要允许来自 Azure 的应用程序连接到 Azure Database for MariaDB 服务器，必须启用 Azure 连接。 例如，为了托管“Azure Web 应用”应用程序或 Azure VM 中运行的应用程序，或者为了从 Azure 数据工厂数据管理网关进行连接。 资源无需在同一虚拟网络 (VNet) 或资源组中，即可使用防火墙规则启用这些连接。 在应用程序尝试从 Azure 连接到数据库服务器时，防火墙会验证是否允许 Azure 连接。 有几种方法可启用这些类型的连接。 如果防火墙设置的开始地址和结束地址都等于 0.0.0.0，则表示允许这些连接。 或者，可以在门户中从“连接安全性”窗格将“允许访问 Azure 服务”选项设为“启用”并单击“保存”。 如果不允许连接尝试，则请求将不会到达 Azure Database for MariaDB 服务器。

> [!IMPORTANT]
> 该选项将防火墙配置为允许来自 Azure 的所有连接，包括来自其他客户的订阅的连接。 选择该选项时，请确保登录名和用户权限将访问权限限制为仅已授权用户使用。
> 

## <a name="manage-existing-firewall-rules-in-the-azure-portal"></a>在 Azure 门户中管理现有防火墙规则
重复这些步骤来管理防火墙规则。
* 若要添加当前计算机，请单击“+ 添加我的 IP”。 单击“保存”以保存更改。
* 若要添加其他 IP 地址，请键入“规则名称”、“起始 IP”和“结束 IP”。 单击“保存”以保存更改。
* 若要修改现有规则，单击规则中的任意字段并修改。 单击“保存”以保存更改。
* 若要删除现有规则，请单击省略号 […]，然后单击“删除”。 单击“保存”以保存更改。

<!--
## Next steps
 - Similarly, you can script to [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](howto-manage-firewall-using-cli.md).
- For help in connecting to an Azure Database for MariaDB server, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md) -->