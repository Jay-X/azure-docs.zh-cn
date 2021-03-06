---
title: PowerShell 示例 - 创建 Azure SQL 数据库托管实例 | Microsoft Docs
description: 创建 Azure SQL 数据库托管实例的 Azure PowerShell 示例脚本
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: PowerShell
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: ''
manager: craigg
ms.date: 01/17/2019
ms.openlocfilehash: bbb22f8d5eab3e60eba13bb642edb8f8d190cf67
ms.sourcegitcommit: 9f07ad84b0ff397746c63a085b757394928f6fc0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/17/2019
ms.locfileid: "54387942"
---
# <a name="use-powershell-with-azure-resource-manager-template-to-create-an-azure-sql-database-managed-instance"></a>将 PowerShell 与 Azure 资源管理器模板配合使用来创建 Azure SQL 数据库托管实例

可以使用 Azure PowerShell 库和 Azure 资源管理器模板来创建 Azure SQL 数据库托管实例。 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]
[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

如果选择在本地安装并使用 PowerShell，则本教程需要 Azure PowerShell 模块 5.7.0 或更高版本。 运行 `Get-Module -ListAvailable AzureRM` 即可查找版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](/powershell/azure/install-az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzureRmAccount` 来创建与 Azure 的连接。

Azure PowerShell 命令可以使用预定义的 Azure 资源管理器模板启动部署。 可以在模板中指定以下属性：
- 实例名称
- SQL 管理员用户名和密码。 
- 实例的大小（内核数和最大存储大小）。
- 要将实例放置在其中的 VNet 和子网。
- 实例的服务器级排序规则（预览版）。

实例名称、SQL 管理员用户名、VNet/子网和排序规则以后无法更改。 其他实例属性可以更改。

## <a name="prerequisites"></a>先决条件

此示例假定已为托管实例[创建了有效的网络环境](../sql-database-managed-instance-create-vnet-subnet.md)或[修改了现有的 VNet](../sql-database-managed-instance-configure-vnet-subnet.md)。 此示例使用 cmdlet [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) 和 [Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork)，因此请确保已安装以下 PowerShell 模块：

```
Install-Module AzureRM.Network
Install-Module AzureRM.Resources
```

## <a name="azure-resource-manager-template"></a>Azure 资源管理器模板

应当在表示模板的文件中放置将用来创建实例的以下内容：
```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "instance": {
            "type": "string"
        },
        "user": {
            "type": "string"
        },
        "pwd": {
            "type": "securestring"
        },
        "subnetId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('instance')]",
            "location": "West Central US",
            "tags": {
                "Description":"GP Instance with custom instance collation - Serbian_Cyrillic_100_CS_AS"
            },          
            "sku": {
                "name": "GP_Gen4",
                "tier": "GeneralPurpose"
            },
            "properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen4", 
                "collation": "Serbian_Cyrillic_100_CS_AS"
            },
            "type": "Microsoft.Sql/managedInstances",
            "identity": {
                "type": "SystemAssigned"
            },
            "apiVersion": "2015-05-01-preview"
        }
    ]
}
```
我们假设已存在正确配置了子网的 Azure VNet。 如果此假设不属实，请使用可以独立执行或包括在此模板中的单独 [Azure 资源管理器模板](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment)来准备网络环境。

将此文件的内容保存为 .json 文件，将该文件路径置于以下 PowerShell 脚本中，并在脚本中更改对象的名称： 

```powershell
$subscriptionId = "ed827499-xxxx-xxxx-xxxx-xxxxxxxxxx"
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Managed Instance properties
$resourceGroup = "rg_mi"
$location = "West Central US"
$name = "managed-instance-name"
$user = "miSqlAdmin"
$secpasswd = ConvertTo-SecureString "<Put some strong password here>" -AsPlainText -Force

# Network configuration
$vNetName = "my_vnet"
$vNetResourceGroup = "rg_mi_vnet"
$subnetName = "ManagedInstances"
$vNet = Get-AzureRmVirtualNetwork -Name $vNetName -ResourceGroupName $vNetResourceGroup
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -VirtualNetwork $vNet
$subnetId = $subnet.Id

# Deploy Instance using Azure Resource Manager template:
New-AzureRmResourceGroupDeployment  -Name MyDeployment -ResourceGroupName $resourceGroup  `
                                    -TemplateFile 'C:\...\create-managed-instance.json' `
                                    -instance $name -user $user -pwd $secpasswd -subnetId $subnetId
```
成功运行该脚本后，可以通过所有 Azure 服务和配置的 IP 地址访问 SQL 数据库。 

## <a name="next-steps"></a>后续步骤

有关 Azure PowerShell 的详细信息，请参阅 [Azure PowerShell 文档](/powershell/azure/overview)。

可以在 [Azure SQL 数据库 PowerShell 脚本](../sql-database-powershell-samples.md)中找到更多 SQL 数据库 PowerShell 脚本示例。



