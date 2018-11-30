---
title: Azure Log Analytics 中的网络性能监视器解决方案中的性能监视器功能 | Microsoft Docs
description: 借助网络性能监视器中的性能监视器功能可以监视网络中各个位置的网络连接。 可以监视云部署和本地位置、多个数据中心和分支机构、任务关健型多层应用程序或微服务。
services: log-analytics
documentationcenter: ''
author: abshamsft
manager: carmonm
editor: ''
ms.assetid: 5b9c9c83-3435-488c-b4f6-7653003ae18a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/20/2018
ms.author: abshamsft
ms.component: ''
ms.openlocfilehash: 8b4ed19ede70c3c9b768cfd368e22b0df3e71212
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "52430532"
---
# <a name="network-performance-monitor-solution-performance-monitoring"></a>网络性能监视器解决方案：性能监视

借助[网络性能监视器](network-performance-monitor.md)中的性能监视器功能可以监视网络中各个位置的网络连接。 可以监视云部署和本地位置、多个数据中心和分支机构、任务关健型多层应用程序或微服务。 使用性能监视器，可以在用户产生抱怨之前检测到网络问题。 主要优势包括： 

- 跨各种子网监视数据丢失和延迟情况并设置警报。
- 监视网络上的所有路径（包括冗余路径）。
- 对难以复现且在特定时间点出现的暂时性网络问题进行故障排除。
- 确定网络上导致性能下降的具体分段。
- 监视网络运行状况，不需要 SNMP。


![网络性能监视器](media/network-performance-monitor-performance-monitor/npm-performance-monitor.png)

## <a name="configuration"></a>配置
若要打开网络性能监视器的配置，请打开[网络性能监视器解决方案](network-performance-monitor.md)并选择“配置”。

![配置网络性能监视器](media/network-performance-monitor-performance-monitor/npm-configure-button.png)

### <a name="create-new-networks"></a>新建网络

网络性能监视器中的网络是子网的逻辑容器。 它可以根据需要帮助你对网络基础结构的监视进行组织。 可以创建一个具有友好名称的网络，并根据业务逻辑在其中添加子网。 例如，可以创建名为 London 的网络并添加伦敦数据中心内的所有子网。 或者，可以创建名为 *ContosoFrontEnd* 的网络，并将为应用前端提供服务的、名为 Contoso 的所有子网添加到此网络。 该解决方案会自动创建一个默认网络，其中包含在你的环境中发现的所有子网。 

每当创建网络时，都可以向该网站添加子网。 然后从默认网络中删除该子网。 删除某个网络时，会自动将其中的所有子网返回到默认网络。 默认网络充当包含在用户定义的任何网络中的所有子网的容器。 无法编辑或删除默认网络。 它始终保留在系统中。 可以根据需要创建任意数量的自定义网络。 在大多数情况下，会将组织中的子网排列在多个网络中。 应根据业务逻辑创建一个或多个网络，以便对子网进行分组。

若要创建新网络，请执行以下操作：


1. 选择“网络”选项卡。
1. 选择 **添加网络**，并输入网络名称和说明。 
2. 选择一个或多个子网，然后选择“添加”。 
3. 选择“保存”以保存配置。 


### <a name="create-monitoring-rules"></a>创建监视规则 

突破 2 个子网或 2 个网络之间网络连接的性能阈值时，性能监视器会生成运行状况事件。 系统可以自动获知这些阈值。 你也可以提供自定义警报。 系统会自动创建一个默认规则，每当任何一对网络或子网链接之间的丢失或延迟突破系统获知的阈值时，该规则就会生成运行状况事件。 在尚未显式创建任何监视规则之前，此过程可以帮助解决方案监视网络基础结构。 如果启用了默认规则，所有节点都会将综合事务发送到已启用监视的其他所有节点。 默认规则非常适合小型网络。 例如，有少量的服务器在运行微服务，而你想要确保所有服务器相互连接，则默认规则就非常有用。

>[!NOTE]
> 我们建议禁用默认规则并创建自定义监视规则，尤其网络规模较大，需要使用大量的节点进行监视时。 自定义监视规则可以减少解决方案生成的流量，并有助于对网络监视进行组织。

根据业务逻辑创建监视规则。 假设你要监视两个办公地点到总部的网络连接性能。 可将办公地点 1 中的所有子网分组到网络 O1。 然后将办公地点 2 中的所有子网分组到网络 O2。 最后，将总部中的所有子网分组到网络 H 中。创建两个监视规则 - 一个在 O1 与 H 之间，另一个在 O2 与 H 之间。 

若要创建自定义监视规则，请执行以下操作：

1. 在“监视器”选项卡中，选择“添加规则”，并输入规则名称和说明。
2. 从列表中选择要监视的网络或子网链接对。 
3. 从网络下拉列表中选择包含所需子网的网络。 然后从相应的子网下拉列表中选择子网。 若要监视网络链接中的所有子网，请选择“所有子网”。 同样，选择其他所需的子网。 若要从已选项中排除对特定子网链接的监视，请选择“添加例外”。 
4. 选择 ICMP 或 TCP 协议用于执行综合事务。 
5. 如果不希望针对所选项生成运行状况事件，请清除“启用对此规则覆盖的链接进行运行状况监视”。 
6. 选择监视条件。 若要针对运行状况事件生成设置自定义阈值，请输入阈值。 只要条件值超出其针对所选网络或子网对选择的阈值，就会生成运行状况事件。 
7. 选择“保存”以保存配置。 

保存监视规则后，可以选择“创建警报”，将该规则与警报管理集成。 系统将使用搜索查询自动创建警报规则。 其他所需参数会自动填入。 使用警报规则可以收到基于电子邮件的警报，以及网络性能监视器中的现有警报。 警报还能配合 Runbook 触发补救措施，或者，可以使用 Webhook 将警报与现有服务管理解决方案集成。 选择“管理警报”以编辑警报设置。 

现在可以创建多个性能监视器规则或移动到解决方案仪表板来开始使用此功能。

### <a name="choose-the-protocol"></a>选择协议

网络性能监视器使用综合事务来计算数据包丢失和链接延迟等网络性能指标。 为了更好地理解此概念，请考虑一个已连接到网络链接一端的网络性能监视器代理。 此网络性能监视器代理将探测数据包发送至已连接到网络另一端的第二个网络性能监视器代理。 第二个代理使用响应数据包答复。 此过程将重复多次。 通过测量答复数和接收每个答复所花费的时间，第一个网络性能监视器代理将评估链接延迟和数据包丢弃情况。 

这些数据包的格式、大小和序列由在创建监视规则时选择的协议决定。 根据数据包协议，中间网络设备（例如路由器和交换机）可能以不同方式处理这些数据包。 因此，协议选择将影响结果的准确性。 选择的协议还决定了是否在部署网络性能监视器解决方案后必须执行任何手动步骤。 

网络性能监视器提供 ICMP 与 TCP 协议之间的选择，以便于执行综合事务。 如果在创建综合事务规则时选择 ICMP，网络性能监视器代理将使用 ICMP ECHO 消息来计算与网络延迟和数据包丢失相关的数据。 ICMP ECHO 使用传统 ping 实用工具发送的同一消息。 当使用 TCP 作为协议时，网络性能监视器代理通过网络发送 TCP SYN 数据包。 执行此步骤后，将完成 TCP 握手，然后使用 RST 数据包删除连接。 

在选择协议之前，请考虑以下信息： 

* **发现多个网络路由。** 发现多个路由时，TCP 更为准确，并且在每个子网中需要使用的代理更少。 例如，一个或两个使用 TCP 的代理可以发现子网之间的所有冗余路径。 需要多个使用 ICMP 的代理才能实现类似效果。 使用 ICMP，如果两个子网之间有许多路由，则源或目标子网中需要 5N 个以上的代理。

* **结果的准确性。** 路由器和交换机往往将较低的优先级分配给 ICMP ECHO 数据包（与 TCP 数据包相比）。 在某些情况下，当网络设备负载很重时，TCP 获取的数据将更准确地反映应用程序遇到的丢失和延迟情况。 这是因为大部分应用程序流量都是通过 TCP 传送的。 在这种情况下，与 TCP 相比，ICMP 提供的结果准确性较低。 

* **防火墙配置。** TCP 协议要求 TCP 数据包发送到目标端口。 网络性能监视器代理使用的默认端口为 8084。 可以在配置代理时更改此端口。 请确保网络防火墙或网络安全组 (NSG) 规则（在 Azure 中）允许该端口上的流量。 还需要确保安装代理的计算机上的本地防火墙已配置为允许此端口上的流量。 可以使用 PowerShell 脚本配置运行 Windows 的计算机上的防火墙规则，但需要手动配置网络防火墙。 相反，ICMP 不使用端口运行。 多数企业方案中都允许 ICMP 流量通过防火墙，以便于使用 ping 实用工具等网络诊断工具。 如果可以从一台计算机 ping 另一台计算机，则可以使用 ICMP 协议而无需手动配置防火墙。

>[!NOTE] 
> 某些防火墙可能会阻止 ICMP，导致重新传输，因而在安全信息和事件管理系统中生成大量的事件。 请确保选择的协议未被网络防火墙或 NSG 阻止。 否则网络性能监视器无法监视网段。 我们建议使用 TCP 进行监视。 如果存在如下所述的情况，因而无法使用 TCP，请使用 ICMP： 
>
> - 使用基于 Windows 客户端的节点，因为 Windows 客户端中不允许 TCP 原始套接字。
> - 网络防火墙或 NSG 会阻止 TCP。
> - 不知道如何切换协议。

如果在部署期间选择使用 ICMP，可以随时通过编辑默认监视规则来切换到 TCP。

1. 转到 ”**网络性能**”   > ” **监视**”   > ” **配置**”   > ” **监视**” 。 然后选择  **“默认规则”**。 
2. 滚动到“协议”部分，并选择要使用的协议。 
3. 选择“保存”以应用设置。 

即使默认规则使用特定协议，也可以使用其他协议创建新规则。 甚至可以创建混合规则，其中一些规则使用 ICMP，另一些规则使用 TCP。 

## <a name="walkthrough"></a>演练 

现在，我们简单调查一下某个运行状况事件的根本原因。

在解决方案仪表板上，某个运行状况事件显示网络链接不正常。 若要调查问题，请选择“正受到监视的网络链接”磁贴。

深化页上显示 **DMZ2-DMZ1** 网络链接不正常。 选择此网络链接对应的“查看子网链接”。 


深化页显示 **DMZ2-DMZ1** 网络链接中的所有子网链接。 这两个子网链接的延迟已超过阈值，使网络链接不正常。 还可以看到这两个子网链接的延迟趋势。 使用图表中的时间选择控件可以重点关注所需的时间范围。 可以查看延迟达到其峰值时的当天时间。 若要调查问题，以后可以搜索日志来查询此时间段。 选择“查看节点链接”以进一步深化。 
 
 ![“子网链接”页](media/network-performance-monitor-performance-monitor/subnetwork-links.png) 

与上一页类似，特定子网链接的挖掘页面会列出其构成节点链接。 可以在此处执行与上一步类似的操作。 选择“查看拓扑”可查看两个节点之间的拓扑。 
 
 ![“节点链接”页](media/network-performance-monitor-performance-monitor/node-links.png) 

两个所选节点之间的所有路径都绘制在拓扑图中。 可以在拓扑图上可视化两个节点之间路由的逐跳拓扑。 它清晰地呈现两个节点之间存在多少个路由，以及数据包会采用哪条路径。 网络性能瓶颈以红色显示。 若要定位到发生问题的网络连接或网络设备，请查看拓扑图上的红色元素。 

 ![包含拓扑图的拓扑仪表板](media/network-performance-monitor-performance-monitor/topology-dashboard.png) 

可在“操作”窗格中查看每个路径中的丢失情况、延迟和跃点数。 滚动条可用于查看不正常路径的详细信息。 使用筛选器可以选择包含不正常跃点的路径，以便只绘制所选路径的拓扑。 使用鼠标滚轮可以放大或缩小拓扑图。 

在下图中，网络特定部分的问题区域的根本原因出现在红色路径和跃点中。 选择拓扑图中的某个节点会呈现该节点的属性，包括 FQDN 和 IP 地址。 选择某个跃点会显示该跃点的 IP 地址。 
 
![已在其中选择节点属性的拓扑图](media/network-performance-monitor-performance-monitor/topology-dashboard-root-cause.png) 

## <a name="next-steps"></a>后续步骤
[搜索日志](../../log-analytics/log-analytics-queries.md)以查看详细的网络性能数据记录。