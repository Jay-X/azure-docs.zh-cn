---
title: 更新 Azure Stack MySQL 资源提供程序 | Microsoft Docs
description: 了解如何更新 Azure Stack MySQL 资源提供程序。
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2019
ms.author: jeffgilb
ms.reviewer: jiahan
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: f38d27c6f82533265705ff5483bfe835c81c9ce6
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/07/2019
ms.locfileid: "55817285"
---
# <a name="update-the-mysql-resource-provider"></a>更新 MySQL 资源提供程序 

*适用于：Azure Stack 集成系统。*

新的 MySQL，Azure Stack 构建系统时，可能会释放资源提供程序适配器进行更新。 虽然现有的适配器可以继续使用，但仍建议尽快更新到最新的内部版本。 

从 MySQL 资源提供程序版本 1.1.33.0 发行版开始，更新是累积的并且不需要在其中发布; 的顺序安装只要您从版本 1.1.24.0 或更高版本。 例如，如果正在运行的 MySQL 资源提供程序版本 1.1.24.0，则可以升级到版本 1.1.33.0 或更高版本而无需首先安装版本 1.1.30.0。 若要查看可用的资源提供程序版本和 Azure Stack 支持的版本，请参阅中的版本列表[部署的资源提供程序先决条件](./azure-stack-mysql-resource-provider-deploy.md#prerequisites)。

若要更新资源提供程序，请使用 **UpdateMySQLProvider.ps1** 脚本。 该过程是类似于用于安装资源提供程序，如中所述部署资源提供程序部分中的这篇文章的过程。 资源提供程序的下载包中提供此脚本。 

 > [!IMPORTANT]
 > 在升级前的资源提供程序，查看发行说明以了解有关新功能、 修复和可能会影响你的部署任何已知的问题的信息。

## <a name="update-script-processes"></a>更新脚本过程

**UpdateMySQLProvider.ps1** 脚本可使用最新的资源提供程序代码创建新的 VM，并可将设置从旧 VM 迁移到新 VM。 迁移的设置包括数据库和宿主服务器信息，以及必需的 DNS 记录。 

>[!NOTE]
>建议从市场管理下载最新的 Windows Server 2016 Core 映像。 如需安装更新，可以将**单个** MSU 包放置在本地依赖项路径中。 如果此位置中有多个 MSU 文件，则脚本将失败。

此脚本需要使用的参数正是针对 DeployMySqlProvider.ps1 脚本进行描述的参数。 请同样在此处提供证书。  


## <a name="update-script-parameters"></a>更新脚本参数 
在运行时，可以指定以下参数从命令行**UpdateMySQLProvider.ps1** PowerShell 脚本。 如果未指定参数或任何参数验证失败，系统会提示提供所需的参数。 

| 参数名称 | 描述 | 注释或默认值 | 
| --- | --- | --- | 
| **CloudAdminCredential** | 访问特权终结点时所需的云管理员凭据。 | _必需_ | 
| **AzCredential** | Azure Stack 服务管理员帐户的凭据。 使用部署 Azure Stack 时所用的相同凭据。 | _必需_ | 
| **VMLocalCredential** |SQL 资源提供程序 VM 的本地管理员帐户的凭据。 | _必需_ | 
| **PrivilegedEndpoint** | 特权终结点的 IP 地址或 DNS 名称。 |  _必需_ | 
| **AzureEnvironment** | 用于部署 Azure Stack 的服务管理员帐户的 Azure 环境。 仅对于 Azure AD 部署是必需的。 受支持的环境名称**AzureCloud**， **AzureUSGovernment**，或如果使用中国 Azure AD **AzureChinaCloud**。 | AzureCloud |
| **DependencyFilesLocalPath** | 同样必须将证书 .pfx 文件放在此目录中。 | _可选_（对于多节点部署是_必需_的） | 
| **DefaultSSLCertificatePassword** | .pfx 证书的密码。 | _必需_ | 
| **MaxRetryCount** | 操作失败时，想要重试每个操作的次数。| 2 | 
| **RetryDuration** | 每两次重试的超时间隔（秒）。 | 120 | 
| **卸载** | 删除资源提供程序和所有关联的资源（请参阅下面的注释）。 | “否” | 
| **DebugMode** | 防止在失败时自动清除。 | “否” | 
| **AcceptLicense** | 跳过接受 GPL 许可条款的提示。  (https://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

## <a name="update-script-example"></a>更新脚本示例
下面是使用的示例*UpdateMySQLProvider.ps1*可以从提升的 PowerShell 控制台运行的脚本。 请务必根据需要更改变量的信息和密码：  

> [!NOTE] 
> 此更新过程仅适用于集成系统。 

```powershell 
# Install the AzureRM.Bootstrapper module, set the profile and install the AzureStack module
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2018-03-01-hybrid -Force
Install-Module -Name AzureStack -RequiredVersion 1.5.0

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time. 
$domain = "AzureStack" 

# For integrated systems, use the IP address of one of the ERCS virtual machines 
$privilegedEndpoint = "AzS-ERCS01" 

# Provide the Azure environment used for deploying Azure Stack. Required only for Azure AD deployments. Supported environment names are AzureCloud, AzureUSGovernment, or AzureChinaCloud. 
$AzureEnvironment = "<EnvironmentName>"

# Point to the directory where the resource provider installation files were extracted. 
$tempDir = 'C:\TEMP\MYSQLRP' 

# The service admin account (can be Azure Active Directory or Active Directory Federation Services). 
$serviceAdmin = "admin@mydomain.onmicrosoft.com" 
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass) 
 
# Set credentials for the new resource provider VM. 
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass) 
 
# And the cloudadmin credential required for privileged endpoint access. 
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass) 

# Change the following as appropriate. 
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force 
 
# Change directory to the folder where you extracted the installation files. 
# Then adjust the endpoints. 
$tempDir\UpdateMySQLProvider.ps1 -AzCredential $AdminCreds ` 
-VMLocalCredential $vmLocalAdminCreds ` 
-CloudAdminCredential $cloudAdminCreds ` 
-PrivilegedEndpoint $privilegedEndpoint ` 
-AzureEnvironment $AzureEnvironment `
-DefaultSSLCertificatePassword $PfxPass ` 
-DependencyFilesLocalPath $tempDir\cert ` 
-AcceptLicense 
```  

## <a name="next-steps"></a>后续步骤
[维护 MySQL 资源提供程序](azure-stack-mysql-resource-provider-maintain.md)
