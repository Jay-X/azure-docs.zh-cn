---
title: Azure Stack 中的 DNS | Microsoft Docs
description: 使用 Azure Stack 中的 DNS
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 822fd40b2fc707efe34d4081c3c49e5f71621cdb
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245535"
---
# <a name="using-dns-in-azure-stack"></a>使用 Azure Stack 中的 DNS

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

Azure Stack 支持以下域名系统 (DNS) 功能：

* DNS 主机名解析
* 使用 API 创建和管理 DNS 区域和记录

## <a name="support-for-dns-hostname-resolution"></a>支持 DNS 主机名解析

可以为公用 IP 资源指定一个 DNS 域名标签。 Azure Stack 使用 **domainnamelabel.location.cloudapp.azurestack.external** 作为标签名称并将其映射到 Azure Stack 托管 DNS 服务器中的公共 IP 地址。

例如，如果创建公共 IP 资源时**contoso**作为在本地 Azure Stack 位置中，一个域名标签[完全限定的域名 (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) **contoso.local.cloudapp.azurestack.external**解析为该资源的公共 IP 地址。 可以使用此 FQDN 创建自定义域 CNAME 记录，该记录指向 Azure Stack 中的公共 IP 地址。

若要了解有关名称解析的详细信息，请参阅 [DNS 解析](../../dns/dns-for-azure-services.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)一文。

> [!IMPORTANT]
> 创建的每个域名标签在其 Azure Stack 位置中必须是唯一的。

以下屏幕截图显示了使用门户创建公共 IP 地址时的“创建公共 IP 地址”对话框：

![创建公共 IP 地址](media/azure-stack-whats-new-dns/image01.png)

### <a name="example-scenario"></a>示例方案

你有一个用于处理来自 Web 应用程序的请求的负载均衡器。 负载均衡器的后面是一个在一台或多台虚拟机上运行的网站。 可以使用 DNS 名称而非 IP 地址来访问进行了负载均衡的网站。

## <a name="create-and-manage-dns-zones-and-records-using-the-api"></a>使用 API 创建和管理 DNS 区域和记录

可以在 Azure Stack 中创建和管理 DNS 区域和记录。

Azure Stack 使用与 Azure DNS API 一致的 API 提供与 Azure 类似的 DNS 服务。  通过将域托管在 Azure Stack DNS 中，可以使用相同的凭据、API 和工具来管理 DNS 记录。 还可以使用与其他 Azure 服务相同的计费和支持功能。

Azure Stack DNS 的基础结构比 Azure 的更为精简。 Azure Stack 部署的大小和位置会影响 DNS 的作用域、规模和性能。 这也意味着性能、 可用性、 全局分发和高可用性可以改变部署的。

## <a name="comparison-with-azure-dns"></a>与 Azure DNS 比较

Azure Stack 中的 DNS 类似于 DNS 在 Azure 中，但有几个重要的例外情况：

* **不支持 AAAA 记录**:Azure Stack 不支持 AAAA 记录，因为 Azure Stack 不支持 IPv6 地址。 这是 Azure DNS 与 Azure Stack DNS 之间的主要差异。

* **不是多租户**:Azure Stack 中的 DNS 服务不是多租户的。 各个租户不能创建相同的 DNS 区域。 仅首个订阅尝试创建区域会成功，后续请求都会失败。 这是 Azure DNS 与 Azure Stack DNS 之间的主要差异。

* **标记、 元数据和 Etag**:Azure Stack DNS 在处理标记、元数据、Etag 和限制的方式方面也有一些细微差异。

若要了解有关 Azure DNS 的更多信息，请参阅 [DNS 区域和记录](../../dns/dns-zones-records.md)。

### <a name="tags"></a>标记

Azure Stack DNS 支持在 DNS 区域资源上使用 Azure 资源管理器标记。 它不支持标记上 DNS 记录集，作为替代方法，尽管**元数据**下一节中所述，在 DNS 记录集上支持。

### <a name="metadata"></a>元数据

作为记录集标记的替代方法，Azure Stack DNS 支持使用“元数据”批注记录集。 与标记相类似，通过元数据可将名称/值对与每个记录集相关联。 这非常有用，例如可用于记录每个记录集的用途。 与标记不同的是，不能使用元数据提供 Azure 帐单的筛选视图，且不能在 Azure 资源管理器策略中指定元数据。

### <a name="etags"></a>Etag

假设两个人或两个进程尝试同时修改一条 DNS 记录。 哪一个占先？ 占先方是否知道他们/它们覆盖了其他人/进程创建的更改？

Azure Stack DNS 使用 Etag 来安全地处理对同一资源的并发更改。 Etag 与 Azure 资源管理器“标记”不同。 每个 DNS 资源（区域或记录集）都有与其相关联的 Etag。 检索资源时，还会检索其 Etag。 更新资源时，可以选择传递回 Etag 以便 Azure Stack DNS 可以验证服务器上的 Etag 是否匹配。 由于对资源的每次更新都会导致重新生成 Etag，Etag 不匹配表示发生了并发更改。 当创建新的资源时也可以使用 Etag，以确保该资源不存在。

默认情况下，Azure Stack DNS PowerShell cmdlet 使用 Etag 来阻止对区域和记录集的并发更改。 您可以使用可选`-Overwrite`切换来取消 Etag 检查，这将导致发生覆盖任何并发更改。

Etag 是在 Azure Stack DNS REST API 级别使用 HTTP 标头指定的。 下表介绍了它们的行为：

| 标头 | 行为|
|--------|---------|
| 无   | PUT 始终成功（没有 Etag 检查）|
| If-match| 只有当资源存在并且 Etag 匹配时，PUT 才会成功|
| If-match *| 只有当资源存在时，PUT 才会成功|
| If-none-match *| 只有当资源不存在时，PUT 才会成功|

### <a name="limits"></a>限制

使用 Azure Stack DNS 时，以下默认限制适用：

| 资源| 默认限制|
|---------|--------------|
| 每个订阅的区域数| 100|
| 每个区域的记录集数| 5000|
| 每个记录集的记录数| 20|

## <a name="next-steps"></a>后续步骤

- [适用于 Azure Stack 的 iDNS 简介](azure-stack-understanding-dns.md)
