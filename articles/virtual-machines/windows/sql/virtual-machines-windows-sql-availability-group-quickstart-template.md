---
title: 使用 Azure 快速入门模板在 SQL Server VM 上为 Always On 可用性组创建 WSFC、侦听程序和配置 ILB
description: 使用 Azure 快速入门模板可以创建群集、将 SQL VM 加入群集、创建侦听程序和配置 ILB，并以此简化在 Azure 中为 SQL Server VM 创建可用性组的过程。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/04/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 9be8717bc9b1d15a59486edf206dd0657a711c06
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54360434"
---
# <a name="create-wsfc-listener-and-configure-ilb-for-an-always-on-availability-group-on-a-sql-server-vm-with-azure-quickstart-template"></a>使用 Azure 快速入门模板在 SQL Server VM 上为 Always On 可用性组创建 WSFC、侦听程序和配置 ILB
本文介绍如何使用 Azure 快速入门模板来部分自动化在 Azure 中为 SQL Server 虚拟机部署 Always On 可用性组配置的过程。 此过程使用两个 Azure 快速入门模板。 

   | 模板 | Description |
   | --- | --- |
   | [101-sql-vm-ag-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) | 创建 Windows 故障转移群集并将 SQL Server VM 加入其中。 |
   | [101-sql-vm-aglistener-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) | 创建可用性组侦听程序并配置内部负载均衡器。 |
   | &nbsp; | &nbsp; |

其他可用性组配置部分（例如创建可用性组，以及创建内部负载均衡器）必须手动完成。 本文提供自动和手动步骤的顺序。
 

## <a name="prerequisites"></a>先决条件 
若要使用快速入门模板自动设置 Always On 可用性组，必须满足以下先决条件： 
- 一个 [Azure 订阅](https://azure.microsoft.com/free/)。
- 一个具有[域控制器](https://docs.microsoft.com/azure/architecture/reference-architectures/identity/adds-forest)的资源组。 
- Azure 中的一个或多个已加入域的 VM，它们[运行 SQL Server 2016 Enterprise Edition（或更高版本）](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision)，位于[已注册到 SQL VM 资源提供程序](#register-existing-sql-vm-with-new-resource-provider)的同一个可用性集或可用性区域中。  

## <a name="register-existing-sql-vm-with-new-resource-provider"></a>将现有 SQL VM 注册到新的资源提供程序
由于这些可用性组 Azure 快速入门模板依赖于 SQL VM 资源提供程序 (Microsoft.SqlVirtualMachine)，因此，现有的 SQL Server VM 必须已注册到 SQL VM 资源提供程序。 如果你的 SQL Server VM 是在 2018 年 12 月之后创建的，请跳过此步骤，因为此日期之后创建的 SQL Server VM 会自动注册。 本部分提供使用 Azure 门户注册到提供程序的步骤，但你也可以使用 [PowerShell](virtual-machines-windows-sql-ahb.md#powershell)。 

  >[!IMPORTANT]
  > 如果删除了 SQL Server VM 资源，则会回退到映像的硬编码许可证设置。 

1. 打开 Azure 门户，导航到“所有服务”。 
1. 导航到“订阅”，选择感兴趣的订阅。  
1. 在“订阅”边栏选项卡中，导航到“资源提供程序”。 
1. 在筛选器中键入 `sql`，以便显示与 SQL 相关的资源提供程序。 
1. 根据所需操作为 **Microsoft.SqlVirtualMachine** 提供程序选择“注册”、“重新注册”或“取消注册”。 

  ![修改提供程序](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

## <a name="step-1---create-the-wsfc-and-join-sql-server-vms-to-the-cluster-using-quickstart-template"></a>步骤 1 - 使用快速入门模板创建 WSFC 并将 SQL Server VM 加入群集 
将 SQL Server VM 注册到新的 SQL VM 资源提供程序后，可将 SQL Server VM 加入 *SqlVirtualMachineGroup*。 此资源定义 Windows 故障转移群集的元数据，包括版本、完全限定的域名、用于管理群集的 AD 帐户，以及用作云见证的存储帐户。 将 SQL Server VM 添加到 *SqlVirtualMachineGroup* 会启动 Windows 故障转移群集服务，并将 SQL Server VM 加入群集。 此步骤可以使用 **101-sql-vm-ag-setup** 快速入门模板来自动完成，具体步骤如下：

1. 导航到 [**101-sql-vm-ag-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-ag-setup) 快速入门模板，并选择“部署到 Azure”在 Azure 门户中启动该模板。
1. 填写必填字段，以配置 Windows 故障转移群集元数据。 可选字段可以留空。

    下表显示了模板的所需值： 

   | **字段** | 值 |
   | --- | --- |
   | **订阅** |  SQL Server VM 所在的订阅。 |
   |**资源组** | SQL Server VM 所在的资源组。 | 
   |**故障转移群集名称** | 新 Windows 故障转移群集的所需名称。 |
   | **现有 VM 列表** | 要加入到可用性组，因而要加入到此新群集的 SQL Server VM。 请用逗号和空格分隔这些值（例如：SQLVM1, SQLVM2）。 |
   | **SQL Server 版本** | 从下拉列表中选择 SQL Server VM 的 SQL Server 版本。 目前仅支持 SQL 2016 和 SQL 2017 映像。 |
   | **现有的完全限定域名** | SQL Server VM 所在的域的现有 FQDN。 |
   | **现有域帐户** | 对 SQL Server 拥有 sysadmin 访问权限的现有域帐户。 | 
   | **域帐户密码** | 上述域帐户的密码。 | 
   | **现有 SQL 服务帐户** | 用于控制 SQL Server 服务的域用户帐户。 可以使用 [**SQL Server 配置管理器**](https://docs.microsoft.com/sql/relational-databases/sql-server-configuration-manager?view=sql-server-2017)找到此信息。 |
   | **SQL 服务密码** | 用于控制 SQL Server 服务的域用户帐户使用的密码。 |
   | &nbsp; | &nbsp; |

1. 如果你同意条款和条件，请选中“我同意上述条款和条件”旁边的复选框，然后选择“购买”以完成快速入门模板部署。 
1. 若要监视部署，请通过顶部导航标题中的“通知”钟形图标选择部署，或者在 Azure 门户中导航到你的**资源组**，在“设置”字段中选择“部署”，然后选择“Microsoft.Template”部署。 

## <a name="step-2---manually-create-the-availability-group"></a>步骤 2 - 手动创建可用性组 
像平时一样使用 [PowerShell](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell?view=sql-server-2017)、[SQL Server Management Studio](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio?view=sql-server-2017) 或 [Transact-SQL](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/create-an-availability-group-transact-sql?view=sql-server-2017) 手动创建可用性组。 

  >[!IMPORTANT]
  > 此时请**不要**创建侦听程序，因为在步骤 4 中，**101-sql-vm-aglistener-setup** 快速入门模板会自动创建侦听程序。 

## <a name="step-3---manually-create-the-internal-load-balancer-ilb"></a>步骤 3 - 手动创建内部负载均衡器 (ILB)
Always On 可用性组 (AG) 侦听程序需要一个内部 Azure 负载均衡器 (ILB)。 ILB 为 AG 侦听程序提供“浮动”IP 地址，可以加快故障转移和重新连接的速度。 如果可用性组中的 SQL Server VM 属于同一个可用性集，则你可以使用基本负载均衡器；否则，需要使用标准负载均衡器。  **ILB 应与 SQL Server VM 实例位于同一 vNet 中。** 只需创建 ILB，剩余的配置（例如后端池、运行状况探测和负载均衡规则）将在步骤 4 中由 **101-sql-vm-aglistener-setup** 快速入门模板进行处理。 

1. 在 Azure 门户中，打开包含 SQL Server 虚拟机的资源组。 
2. 在资源组中，单击“添加”。
3. 搜索“负载均衡器”，并在搜索结果中选择 **Microsoft** 发布的“负载均衡器”。
4. 在“负载均衡器”边栏选项卡上，单击“创建”。
5. 在“创建负载均衡器”对话框中配置负载均衡器，如下所示：

   | 设置 | 值 |
   | --- | --- |
   | **名称** |表示负载均衡器的文本名称。 例如 **sqlLB**。 |
   | 类型 |**内部**：大多数实施方案使用内部负载均衡器，它可让同一虚拟网络中的应用程序连接到可用性组。  </br> **外部**：可让应用程序通过公共 Internet 连接连接到可用性组。 |
   | **虚拟网络** |选择 SQL Server 实例所在的虚拟网络。 |
   | **子网** |选择 SQL Server 实例所在的子网。 |
   | IP 地址分配 |**静态** |
   | **专用 IP 地址** |指定子网中的某个可用 IP 地址。 在群集上创建侦听器时，将使用此 IP 地址。|
   | **订阅** |如果有多个订阅，可能会显示此字段。 选择要与此资源关联的订阅。 它通常是与可用性组的所有资源相同的订阅。 |
   | **资源组** |选择 SQL Server 实例所在的资源组。 |
   | **位置** |选择 SQL Server 实例所在的 Azure 位置。 |
   | &nbsp; | &nbsp; |

6. 选择“创建”。 


  >[!NOTE]
  > 每个 SQL Server VM 的公共 IP 资源应有一个与标准负载均衡器兼容的标准 SKU。 若要确定 VM 公共 IP 资源的 SKU，请导航到你的**资源组**，选择所需 SQL Server VM 的“公共 IP 地址”资源，并在“概述”窗格的“SKU”下面找到该值。 

## <a name="step-4---create-the-ag-listener-and-configure-the-ilb-with-the-quickstart-template"></a>步骤 4 - 使用快速入门模板创建 AG 侦听程序并配置 ILB

使用 **101-sql-vm-aglistener-setup** 快速入门模板自动创建可用性组侦听程序并配置内部负载均衡器 (ILB)，因为该模板可预配 Microsoft.SqlVirtualMachine/Sql 虚拟机组/可用性组侦听程序资源。 **101-sql-vm-aglistener-setup** 快速入门模板通过 SQL VM 资源提供程序执行以下操作：

 - 为群集和 ILB 配置网络设置。 
 - 配置 ILB 后端池、运行状况探测和负载均衡规则。
 - 使用给定的 IP 地址和名称创建 AG 侦听程序。

若要配置 ILB 并创建 AG 侦听程序，请执行以下操作：
1. 导航到 [**101-sql-vm-aglistener-setup**](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-vm-aglistener-setup) 快速入门模板，并选择“部署到 Azure”在 Azure 门户中启动该模板。
1. 填写必填字段以配置 ILB，并创建可用性组侦听程序。 可选字段可以留空。 

    下表显示了模板的所需值： 

   | **字段** | 值 |
   | --- | --- |
   |**资源组** | SQL Server VM 和可用性组所在的资源组。 | 
   |**现有故障转移群集名称** | SQL Server VM 要加入到的群集的名称。 |
   | **现有 SQL 可用性组**| SQL Server VM 所属的可用性组的名称。 |
   | **现有 VM 列表** | 属于上述可用性组的 SQL Server VM 的名称。 名称应以逗号和空格分隔（例如：SQLVM1, SQLVM2）。 |
   | **现有的完全限定域名** | SQL Server VM 所在的域的现有 FQDN。 |
   | 侦听器 | 想分配给侦听程序的 DNS 名称。 默认情况下，此模板指定名称“aglistener”，但可以更改。 |
   | **侦听程序端口** | 侦听程序要使用的端口。 通常，此端口应为默认端口 1433，这也是此模板指定的端口号。 但是，如果更改了默认端口，则侦听程序端口应改用该值。 | 
   | **现有 vNet** | SQL Server VM 和 ILB 所在的 vNet 的名称。 |
   | **现有子网** | SQL Server VM 内部子网的名称（例如：default）。 若要确定此值，请导航到你的**资源组**，选择你的 **vNet**，选择“设置”窗格下面的“子网”，然后复制“名称”下面的值。 |
   | **现有内部负载均衡器** | 在步骤 3 中创建的 ILB 的名称。 |
   | **探测端口** | ILB 要使用的探测端口。 该模板默认使用 59999，但可以更改此值。 |
   | &nbsp; | &nbsp; |

1. 如果你同意条款和条件，请选中“我同意上述条款和条件”旁边的复选框，然后选择“购买”以完成快速入门模板部署。 
1. 若要监视部署，请通过顶部导航标题中的“通知”钟形图标选择部署，或者在 Azure 门户中导航到你的**资源组**，在“设置”字段中选择“部署”，然后选择“Microsoft.Template”部署。 

  >[!NOTE]
  >如果部署中途失败，则需要使用 PowerShell 手动[删除新建的侦听程序](#remove-availability-group-listener)，然后重新部署 **101-sql-vm-aglistener-setup** 快速入门模板。 

## <a name="remove-availability-group-listener"></a>删除可用性组侦听程序
如果以后需要删除该模板配置的可用性组侦听程序，则必须通过 SQL VM 资源提供程序执行整个操作。 因为侦听程序是通过 SQL VM 资源提供程序注册的，仅仅通过 SQL Server Management Studio 删除它是不够的。 实际上，应该使用 PowerShell 通过 SQL VM 资源提供程序来删除它。 这样会从 SQL VM 资源提供程序中删除 AG 侦听程序元数据，并将侦听程序从可用性组中实际删除。 

以下代码片段同时从 SQL 资源提供程序和可用性组中删除 SQL 可用性组侦听程序： 

```PowerShell
# Remove the AG listener
# example: Remove-AzureRmResource -ResourceId '/subscriptions/a1a11a11-1a1a-aa11-aa11-1aa1a11aa11a/resourceGroups/SQLAG-RG/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/Cluster/availabilitygrouplisteners/aglistener' -Force
Remove-AzureRmResource -ResourceId '/subscriptions/<SubscriptionID>/resourceGroups/<resource-group-name>/providers/Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups/<cluster-name>/availabilitygrouplisteners/<listener-name>' -Force
```
 
## <a name="known-issues-and-errors"></a>已知问题和错误
本部分讨论一些已知问题及其可能的解决方法。 

### <a name="availability-group-listener-for-availability-group-ag-name-already-exists"></a>可用性组“\<AG 名称>”的可用性组侦听程序已存在
AG 侦听程序 Azure 快速入门模板中使用的选定可用性组已包含一个侦听程序，该侦听程序以物理形式在可用性组中存在，或者以元数据形式在 SQL VM 资源提供程序中存在。  使用 [PowerShell](#remove-availability-group-listener) 删除该侦听程序，然后重新部署 **101-sql-vm-aglistener-setup** 快速入门模板。 

### <a name="connection-only-works-from-primary-replica"></a>只能从主要副本进行连接
此行为的原因可能是 **101-sql-vm-aglistener-setup** 模板部署失败，使 ILB 配置处于不一致状态。 验证后端池是否列出可用性集，并且是否存在运行状况探测规则和负载均衡规则。 如果缺少任何一项，则表示 ILB 配置处于不一致状态。 

若要解决此行为，请使用 [PowerShell](#remove-availability-group-listener) 删除侦听程序，通过 Azure 门户删除内部负载均衡器，然后从[步骤 3](#step-3---manually-create-the-internal-load-balanced-ilb) 重新开始。 

### <a name="badrequest---only-sql-virtual-machine-list-can-be-updated"></a>BadRequest - 只能更新 SQL 虚拟机列表
部署 **101-sql-vm-aglistener-setup** 模板时，如果通过 SQL Server Management Studio (SSMS) 删除了侦听程序，但未将其从 SQL VM 资源提供程序中删除，则可能会发生此错误。 通过 SSMS 删除侦听程序不会从 SQL VM 资源提供程序中删除该侦听程序的元数据；必须使用 [PowerShell](#remove-availability-group-listener) 从资源提供程序中删除该侦听程序。 


## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅以下文章： 

* [SQL Server VM 概述](virtual-machines-windows-sql-server-iaas-overview.md)
* [SQL Server VM 常见问题解答](virtual-machines-windows-sql-server-iaas-faq.md)
* [SQL Server VM 定价指南](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server VM 发行说明](virtual-machines-windows-sql-server-iaas-release-notes.md)
* [切换 SQL Server VM 的许可模型](virtual-machines-windows-sql-ahb.md)



