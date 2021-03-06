---
title: 在 Azure Stack 中使用管理门户 |Microsoft Docs
description: Azure Stack 操作员，了解如何使用管理门户。
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/07/2019
ms.author: jeffgilb
ms.reviewer: ''
ms.lastreviewed: 01/07/2019
ms.openlocfilehash: bda4edeb610fd786307a751afc5b397a2f597e54
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2019
ms.locfileid: "55767463"
---
# <a name="quickstart-use-the-azure-stack-administration-portal"></a>快速入门： 使用 Azure Stack 管理门户

*适用于：Azure Stack 集成系统和 Azure Stack 开发工具包*

在 Azure Stack; 中有两个门户在管理门户和用户门户 (有时称为*租户*门户。)Azure Stack 操作员，你将使用管理门户获取日常管理和操作 Azure Stack。

## <a name="access-the-administrator-portal"></a>访问管理员门户

若要访问管理员门户，请浏览到门户 URL，然后使用 Azure Stack 操作员的凭据登录。 对于集成系统，门户 URL 根据 Azure Stack 部署的区域名称和外部完全限定域名 (FQDN) 而有所不同。 在管理门户 URL 始终是相同的 Azure Stack 开发工具包 (ASDK) 部署。 

| 环境 | 管理员门户 URL |   
| -- | -- | 
| ASDK| https://adminportal.local.azurestack.external  |
| 集成系统 | https://adminportal.&lt;*region*&gt;.&lt;*FQDN*&gt; | 
| | |

> [!TIP]
> 对于 ASDK 环境中，您需要首先请确保你可以[连接到开发工具包主机](azure-stack-connect-azure-stack.md)通过远程桌面连接或通过虚拟专用网络 (VPN)。

 ![在管理门户](media/azure-stack-manage-portals/admin-portal.png)

所有 Azure Stack 部署的默认时区都设置为协调世界时 (UTC)。 

在管理员门户中，你可以执行诸如：

* [将 Azure Stack 注册到 Azure](azure-stack-registration.md)
* [填充 marketplace](azure-stack-download-azure-marketplace-item.md)
* [创建计划、 产品/服务和用户的订阅](azure-stack-plan-offer-quota-overview.md)
* [监视运行状况和警报](azure-stack-monitor-health.md)
* [管理 Azure Stack 的更新](azure-stack-updates.md)

“快速入门教程”磁贴提供最常见任务的联机文档链接。

尽管操作员可以在管理门户中创建虚拟机、 虚拟网络和存储帐户等资源，但您应该[登录到用户门户](user/azure-stack-use-portal.md)来创建和测试资源。

>[!NOTE]
>**创建虚拟机**快速入门教程磁贴中的链接，您可以在管理门户中，创建虚拟机，但这仅用于验证是否已成功部署 Azure Stack。

## <a name="understand-subscription-behavior"></a>了解订阅行为

有三个在管理门户中; 默认情况下创建的订阅消耗、 默认提供商和计数。 作为操作员，主要是将使用*默认提供商订阅*。 不能添加任何其他订阅，并在管理门户中使用它们。 

其他订阅是由用户根据计划和产品/服务为其创建在用户门户中创建的。 但是，用户门户不提供任何在管理门户的管理或操作功能的访问权限。

由 Azure 资源管理器的单独实例支持的管理和用户门户。 由于 Azure 资源管理器这种分离，订阅不会跨门户。 例如，如果您，作为 Azure Stack 操作员，登录到用户门户，则无法访问*默认提供商订阅*。 虽然无法访问任何管理功能，但你可以通过提供的公共套餐为自己创建订阅。 只要你登录到用户门户，系统就会将你视为租户用户。

  >[!NOTE]
  >在 ASDK 环境中，如果用户属于同一租户目录作为 Azure Stack 操作员，它们未被阻止在登录到管理门户。 但是，它们不能访问任何管理功能或添加订阅来访问提供了可用于其用户门户中。

## <a name="administration-portal-tips"></a>管理门户提示

### <a name="customize-the-dashboard"></a>自定义仪表板

仪表板包含一组默认磁贴。 可以选择“编辑仪表板”来修改默认仪表板，或者选择“新建仪表板”来添加自定义仪表板。 可以轻松地将磁贴添加到仪表板中。 例如，可以选择“+ 创建资源”，右键单击“套餐 + 计划”，然后选择“固定到仪表板”。

有时可能会在门户中看到空白的仪表板。 若要恢复仪表板，单击**编辑仪表板**，然后右键单击并选择**重置为默认状态**。

### <a name="quick-access-to-online-documentation"></a>快速访问联机文档

若要访问 Azure Stack 操作员文档，请使用的帮助和支持中的管理员门户右上角的图标 （问号）。 将鼠标移至该图标，然后选择“帮助 + 支持”。

### <a name="quick-access-to-help-and-support"></a>快速访问帮助和支持内容

如果管理员门户中，在右上角选择帮助和支持图标 （问号），然后选择**新建支持请求**，将发生以下结果之一：

- 如果使用的是集成系统，此操作会打开一个站点，可在其中直接向 Microsoft 客户支持服务 (CSS) 创建支持票证。 若要了解何时应该获取 Microsoft 支持或原始设备制造商 (OEM) 硬件供应商支持，请参阅[在何处获取支持](azure-stack-manage-basics.md#where-to-get-support)。
- 如果使用 ASDK，此操作将打开[Azure Stack 论坛站点](https://social.msdn.microsoft.com/Forums/home?forum=AzureStack)直接。 我们会定期关注这些论坛。 ASDK 是一个评估环境，因为没有通过 Microsoft CSS 提供官方支持。

### <a name="quick-access-to-the-azure-roadmap"></a>快速访问 Azure 路线图

如果选择**帮助和支持**（问号） 中的管理门户，并选择右上角**Azure 路线图**，新的浏览器选项卡将打开，并将您带到 Azure 的路线图。 通过键入**Azure Stack**中**产品**搜索框中，可以看到所有 Azure Stack 路线图更新。

## <a name="next-steps"></a>后续步骤

[Azure Stack 注册到 Azure](azure-stack-registration.md)并填充[Azure Stack marketplace](azure-stack-marketplace.md)与要为用户提供的项。 
