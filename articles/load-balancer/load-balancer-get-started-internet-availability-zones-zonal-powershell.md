---
title: 创建具有区域前端的负载均衡器 - Azure PowerShell
titlesuffix: Azure Load Balancer
description: 了解如何使用 Azure PowerShell 创建带有区域性前端的标准负载均衡器
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2018
ms.author: kumud
ms.openlocfilehash: 861759eec266f0ab66d30a466c06e7d2ee14bf06
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247151"
---
#  <a name="create-a-standard-load-balancer-with-zonal-frontend-using-azure-powershell"></a>使用 Azure PowerShell 创建具有区域性前端的标准负载均衡器

本文逐步讲解如何使用公共 IP 标准地址创建具有区域性前端的公共[标准负载均衡器](https://aka.ms/azureloadbalancerstandard)。 若要了解可用性区域如何与标准负载均衡器配合工作，请参阅[标准负载均衡器和可用性区域](load-balancer-standard-availability-zones.md)。 

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

> [!NOTE]
> 选择 Azure 资源、区域和 VM 大小系列时可使用可用性区域支持。 有关如何开始使用以及可以尝试将可用性区域用于哪些 Azure 资源、区域和 VM 大小系列的详细信息，请参阅[可用性区域概述](https://docs.microsoft.com/azure/availability-zones/az-overview)。 若需支持，可以在 [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) 上寻求帮助或者 [open an Azure support ticket](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json)（创建 Azure 支持票证）。

## <a name="log-in-to-azure"></a>登录 Azure

使用 `Connect-AzureRmAccount` 命令登录到 Azure 订阅，并按照屏幕上的说明进行操作。

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>创建资源组

使用以下命令创建资源组：

```powershell
New-AzureRmResourceGroup -Name myResourceGroupZLB -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>创建公共 IP 地址 
使用以下命令创建公共 IP 标准：

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroupZLB -Name 'myPublicIPZonal' `
  -Location westeurope -AllocationMethod Static -Sku Standard -zone 1
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>为网站创建前端 IP 配置

使用以下命令创建前端 IP 配置：

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEnd' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>创建后端地址池

使用以下命令创建后端地址池：

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>在端口 80 上创建负载均衡器探测

使用以下命令在端口 80 上为负载均衡器创建运行状况探测：

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>创建负载均衡器规则
 使用以下命令创建负载均衡器规则：

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>创建负载均衡器
使用以下命令创建标准负载均衡器：

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroupZLB -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>后续步骤
- 详细了解[标准负载均衡器和可用性区域](load-balancer-standard-availability-zones.md)。



