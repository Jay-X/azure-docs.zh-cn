---
title: Azure Stack SQL 资源提供程序 1.1.30.0 发行说明 | Microsoft Docs
description: 了解 Azure Stack SQL 资源提供程序最新更新版的信息，包括任何已知问题和下载位置。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: jeffgilb
ms.reviewer: georgel
ms.openlocfilehash: eb330a782681b8923b95157f28732d89c32100d4
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2019
ms.locfileid: "54122366"
---
# <a name="sql-resource-provider-11330-release-notes"></a>SQL 资源提供程序 1.1.33.0 发行说明

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

这些发行说明介绍了改进和 SQL 资源提供程序版本 1.1.33.0 中的已知的问题。

## <a name="build-reference"></a>内部版本参考
下载 SQL 资源提供程序二进制文件，然后运行自解压程序，将内容解压缩到一个临时目录。 资源提供程序有一个相应的 Azure Stack 最低内部版本。 下面列出了安装此 SQL 资源提供程序版本所需的最低 Azure Stack 发行版：

> |最低 Azure Stack 版本|SQL 资源提供程序版本|
> |-----|-----|
> |版本-1811 (1.1811.0.101)|[SQL RP 版本 1.1.33.0](https://aka.ms/azurestacksqlrp11330)|  
> |     |     |

> [!IMPORTANT]
> 在部署最新版本的 SQL 资源提供程序之前，请先将支持的最低 Azure Stack 更新版应用到 Azure Stack 集成系统，或部署最新的 Azure Stack 开发工具包 (ASDK)。

## <a name="new-features-and-fixes"></a>新功能和修复
此 Azure Stack SQL 资源提供程序版本包含以下改进和修复：

### <a name="fixes"></a>修复项
- **SQL 资源提供程序门户扩展可能会选择错误的订阅**。 SQL 资源提供程序使用 Azure 资源管理器调用以确定第一个服务管理员订阅来使用，这可能不是*默认提供商订阅*。 如果发生这种情况，SQL 资源提供程序不会不正常工作。 

- **SQL 宿主服务器不会列出托管的数据库。** 在查看租户资源的 SQL 宿主服务器时，可能不会列出用户创建的数据库。

- **以前的 SQL 资源提供程序 (1.1.30.0) 部署可能会失败，如果未启用 TLS 1.2**。 更新 SQL 资源提供程序 1.1.33.0 部署资源提供程序，更新资源提供程序，或轮换机密时启用 TLS 1.2。 

- **SQL 资源提供程序机密轮换失败**。 轮换机密时导致以下错误代码修复的问题： ` New-AzureRmResourceGroupDeployment - Error: Code=InvalidDeploymentParameterValue; Message=The value of deployment parameter 'StorageAccountBlobUri' is null.`

## <a name="known-issues"></a>已知问题 

- **SQL SKU 最长可能需要在一小时后才显示在门户中**。 创建新的 SQL 数据库时，新建的 SKU 最长可能需要在一小时后才显示出来并可供使用。 

    **解决方法**：无。

- **重复使用 SQL 登录名**。 尝试使用与现有登录名相同的用户名在同一订阅下创建新的 SQL 登录名时，会导致重复使用同一登录名和现有密码的问题。 

    **解决方法**：在同一订阅下创建新登录名时使用不同的用户名，或者在不同订阅下使用相同的用户名创建登录名。

- **共享的 SQL 登录名导致数据不一致**。 如果在同一订阅下为多个 SQL 数据库共享某个 SQL 登录名，则更改登录密码会导致数据不一致。

    **解决方法**：在同一订阅下始终对不同的数据库使用不同的登录名。

- **SQL 资源提供程序无法添加 SQL Server Always On 侦听器**。 使用侦听器 IP 地址的 SQL Server Always On 侦听器时，SQL 资源提供程序 VM 无法解析该侦听器的主机名。

    **解决方法**：确保 DNS 正常将侦听器 IP 解析为侦听器主机名称。

### <a name="known-issues-for-cloud-admins-operating-azure-stack"></a>云管理员操作 Azure Stack 时的已知问题
请参阅 [Azure Stack 发行说明](azure-stack-servicing-policy.md)中的文档。

## <a name="next-steps"></a>后续步骤
[详细了解 SQL 资源提供程序](azure-stack-sql-resource-provider.md)。

[准备部署 SQL 资源提供程序](azure-stack-sql-resource-provider-deploy.md#prerequisites)。

[从旧版升级 SQL 资源提供程序](azure-stack-sql-resource-provider-update.md)。 