---
title: 将 Azure 流量定向到 Azure SQL 数据库和 SQL 数据仓库 | Microsoft Docs
description: 本文档从 Azure 内部或 Azure 外部说明 Azure SQL 数据库和 SQL 数据仓库连接体系结构。
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: de31ab4e617b872239c1b83324e5b8d52b0b4094
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "55469102"
---
# <a name="azure-sql-connectivity-architecture"></a>Azure SQL 连接体系结构

本文介绍 Azure SQL 数据库和 SQL 数据仓库连接体系结构，以及如何使用不同的组件将流量定向到 Azure SQL 实例。 借助这些连接组件，可以通过连接自 Azure 内部的客户端和连接自 Azure 外部的客户端将网络流量定向到 Azure SQL 数据库或 SQL 数据仓库。 本文还提供脚本示例用于更改连接方式，以及提供与更改默认连接设置相关的注意事项。

> [!IMPORTANT]
> **[即将发生的更改] 对于到 Azure SQL Server 的服务终结点连接，`Default` 连接行为会更改为`Redirect`。**
>
> 更改将于 2019 年 1 月 2 日或之前对所有区域有效。
>
> 为了防止在进行此更改时在现有环境中通过服务终结点进行的连接中断，我们使用遥测执行以下操作：
> - 对于在更改前检测到的通过服务终结点进行过访问的服务器，我们将连接类型切换为 `Proxy`。
> - 对于所有其他的服务器，我们会将连接类型切换为 `Redirect`。
>
> 在以下方案中，服务终结点用户可能仍会受影响：
> - 应用程序不常连接到现有的服务器，因此我们的遥测不捕获有关这些应用程序的信息。
> - 自动部署逻辑创建 SQL 数据库服务器（假设服务终结点连接的默认行为是 `Proxy`）
>
> 如果不能建立到 Azure SQL Server 的服务终结点连接，而你怀疑自己受此更改的影响，请验证是否已将连接类型显式设置为 `Redirect`。 如果是这种情况，则必须对区域中属于端口 11000-12000 的 Sql [服务标记](../virtual-network/security-overview.md#service-tags)的所有 Azure IP 地址启用 VM 防火墙规则和网络安全组 (NSG)。 如果这不是适合自己的选项，请将服务器显式切换为 `Proxy`。
> [!NOTE]
> 本主题适用于 Azure SQL 服务器，同时也适用于在 Azure SQL 服务器中创建的 SQL 数据库和 SQL 数据仓库数据库。 为简单起见，在提到 SQL 数据库和 SQL 数据仓库时，本文统称 SQL 数据库。

## <a name="connectivity-architecture"></a>连接体系结构

下图提供了 Azure SQL Database 连接体系结构的高级别概述。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-overview.png)

以下步骤介绍如何建立到 Azure SQL 数据库的连接。

- 客户端连接到网关，后者使用公共 IP 地址并侦听端口 1433。
- 该网关根据有效的连接策略将流量重定向或代理到适当的数据库群集。
- 在数据库群集中，流量转发到相应的 Azure SQL 数据库。

## <a name="connection-policy"></a>连接策略

Azure SQL 数据库支持 SQL 数据库服务器连接策略设置的以下三个选项：

- **重定向（建议）：** 客户端直接与托管数据库的节点建立连接。 若要启用连接，客户端必须允许对区域中端口 11000-12000 的所有 Azure IP 地址而不仅仅是端口 1433 上的 Azure SQL 数据库网关 IP 地址应用出站防火墙规则，并结合[服务标记](../virtual-network/security-overview.md#service-tags)使用网络安全组 (NSG)。 由于数据包会直接发往数据库，因此延迟、吞吐量和性能都会得到改善。
- **代理：** 在此模式下，所有连接都是通过 Azure SQL 数据库网关代理的。 若要启用连接，客户端必须包含只允许 Azure SQL 数据库网关 IP 地址（通常每个区域有两个 IP 地址）的出站防火墙规则。 选择此模式可能导致延迟增大、吞吐量降低，具体取决于工作负荷的性质。 我们强烈建议使用 `Redirect` 连接策略而不要使用 `Proxy` 连接策略，以最大程度地降低延迟和提高吞吐量。
- **默认：** 除非显式将连接策略更改为 `Proxy` 或 `Redirect`，否则，在创建后，此连接策略将在所有服务器上生效。 有效策略取决于连接是源自 Azure 内部（`Redirect`）还是外部（`Proxy`）。

## <a name="connectivity-from-within-azure"></a>从 Azure 内部连接

如果从 Azure 内部连接，则连接默认具有 `Redirect` 连接策略。 `Redirect` 策略是指建立到 Azure SQL 数据库的 TCP 会话连接后，会将 Azure SQL 数据库网关的目标虚拟 IP 更改为群集的目标虚拟 IP，从而将客户端会话重定向到适当的数据库群集。 此后，所有后续数据包绕过 Azure SQL 数据库网关，直接传输到群集。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>从 Azure 外部连接

如果从 Azure 外部连接，则连接默认具有 `Proxy` 连接策略。 `Proxy` 策略是指通过 Azure SQL 数据库网关建立 TCP 会话，并且所有后续数据包通过网关传输。 下图演示了此流量流。

![体系结构概述](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL 数据库网关 IP 地址

若要从本地资源连接到 Azure SQL 数据库，需要允许到你的 Azure 区域的 Azure SQL 数据库网关的出站网络流量。 在 `Proxy` 模式下连接时，连接仅通过网关建立，从本地资源进行连接时这是默认设置。

下表列出了所有数据区域的 Azure SQL 数据库网关的主 IP 和次要 IP。 某些区域中存在两个 IP 地址。 在这些区域中，主 IP 地址是网关的当前 IP 地址，第二个 IP 地址是故障转移 IP 地址。 故障转移地址是我们可能会将服务器移动到该位置以保持服务的高可用性的地址。 对于这些区域，我们建议允许出站到这两个 IP 地址。 第二个 IP 地址由 Microsoft 拥有，并且不侦听任何服务，除非 Azure SQL 数据库激活该地址以接受连接。

| 区域名称 | 主 IP 地址 | 次要 IP 地址 |
| --- | --- |--- |
| 澳大利亚东部 | 13.75.149.87 | 40.79.161.1 |
| 澳大利亚东南部 | 191.239.192.109 | 13.73.109.251 |
| 巴西南部 | 104.41.11.5 | |
| 加拿大中部 | 40.85.224.249 | |
| 加拿大东部 | 40.86.226.166 | |
| 美国中部 | 23.99.160.139 | 13.67.215.62 |
| 中国东部 1 | 139.219.130.35 | |
| 中国东部 2 | 40.73.82.1 | |
| 中国北部 1 | 139.219.15.17 | |
| 中国北部 2 | 40.73.50.0 | |
| 东亚 | 191.234.2.139 | 52.175.33.150 |
| 美国东部 1 | 191.238.6.43 | 40.121.158.30 |
| 美国东部 2 | 191.239.224.107 | 40.79.84.180 * |
| 法国中部 | 40.79.137.0 | 40.79.129.1 |
| 德国中部 | 51.4.144.100 | |
| 德国东北部 | 51.5.144.179 | |
| 印度中部 | 104.211.96.159 | |
| 印度南部 | 104.211.224.146 | |
| 印度西部 | 104.211.160.80 | |
| 日本东部 | 191.237.240.43 | 13.78.61.196 |
| 日本西部 | 191.238.68.11 | 104.214.148.156 |
| 韩国中部 | 52.231.32.42 | |
| 韩国南部 | 52.231.200.86 |  |
| 美国中北部 | 23.98.55.75 | 23.96.178.199 |
| 北欧 | 191.235.193.75 | 40.113.93.91 |
| 美国中南部 | 23.98.162.75 | 13.66.62.124 |
| 东南亚 | 23.100.117.95 | 104.43.15.0 |
| 英国北部 | 13.87.97.210 | |
| 英国南部 1 | 51.140.184.11 | |
| 英国南部 2 | 13.87.34.7 | |
| 英国西部 | 51.141.8.11 | |
| 美国中西部 | 13.78.145.25 | |
| 西欧 | 191.237.232.75 | 40.68.37.158 |
| 美国西部 1 | 23.99.34.75 | 104.42.238.205 |
| 美国西部 2 | 13.66.226.202 | |
||||

\* **注意：***美国东部 2* 还有第三个 IP 地址，即 `52.167.104.0`。

## <a name="change-azure-sql-database-connection-policy"></a>更改 Azure SQL 数据库连接策略

若要更改 Azure SQL 数据库服务器的 Azure SQL 数据库连接策略，请使用 [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) 命令。

- 如果将连接策略设置为 `Proxy`，则所有网络数据包均通过 Azure SQL 数据库网关进行传输。 对于此设置，需要允许仅出站到 Azure SQL 数据库网关 IP。 使用 `Proxy` 设置比使用 `Redirect` 设置的延迟时间更长。
- 如果连接策略设置为 `Redirect`，则所有网络数据包直接向数据库群集传输。 对于此设置，需要允许出站到多个 IP。

## <a name="script-to-change-connection-settings-via-powershell"></a>通过 PowerShell 编写脚本以更改连接设置

> [!IMPORTANT]
> 此脚本需要 [Azure PowerShell 模块](/powershell/azure/install-az-ps)。

以下 PowerShell 脚本演示如何更改连接策略。

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzureRmSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzureRmResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzureRmResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>通过 Azure CLI 编写脚本以更改连接设置

> [!IMPORTANT]
> 此脚本需要 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。

以下 CLI 脚本演示如何更改连接策略。

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>后续步骤

- 有关如何更改 Azure SQL 数据库服务器的 Azure SQL 数据库连接策略的信息，请参阅 [conn-policy](https://docs.microsoft.com/cli/azure/sql/server/conn-policy)。
- 有关使用 ADO.NET 4.5 或更高版本的客户端的 Azure SQL 数据库连接行为的信息，请参阅[用于 ADO.NET 4.5 的非 1433 端口](sql-database-develop-direct-route-ports-adonet-v12.md)。
- 有关常规应用程序开发的概述信息，请参阅[SQL 数据库应用程序开发概述](sql-database-develop-overview.md)。
