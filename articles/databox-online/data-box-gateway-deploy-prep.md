---
title: 有关在 Azure 门户中做好准备以部署 Data Box Gateway 的教程 | Microsoft Docs
description: 本文为有关部署 Azure Data Box Gateway 第一篇教程，涉及到如何在 Azure 门户中做好准备
services: databox-edge-gateway
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: alkohli
ms.openlocfilehash: 6f47606e91ec55bae624527bace81d947c5ea4f9
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2019
ms.locfileid: "55091540"
---
# <a name="tutorial-prepare-to-deploy-azure-data-box-gateway-preview"></a>教程：准备部署 Azure Data Box Gateway（预览版）


本文是有关完全部署 Azure Data Box Gateway 的教程系列的第一篇教程。 本教程介绍如何在 Azure 门户中做好准备，以部署 Data Box Gateway 资源。 

需要有管理员权限才能完成安装和配置过程。 门户准备只需不到 10 分钟的时间。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建新资源
> * 下载虚拟设备映像
> * 获取激活密钥

如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。


> [!IMPORTANT]
> - Data Box Gateway 为预览版。 在订购和部署此解决方案之前，请查看 [Azure 预览版服务的条款](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。 

### <a name="get-started"></a>入门

若要部署 Data Box Gateway，请按顺序参阅以下教程。

| **#** | **此步骤的内容** | **使用这些文档** |
| --- | --- | --- | 
| 1. |**[在 Azure 门户中做好部署 Data Box Gateway 的准备](data-box-gateway-deploy-prep.md)** |在预配 Data Box Gateway 虚拟设备之前创建并配置 Data Box Gateway 资源。 |
| 2. |**[在 Hyper-V 中预配 Data Box Gateway](data-box-gateway-deploy-provision-hyperv.md)** <br><br><br>**[在 VMware 中预配 Data Box Gateway](data-box-gateway-deploy-provision-vmware.md)**|对于 Hyper-V，可在 Windows Server 2016 或 Windows Server 2012 R2 上运行 Hyper-V 的主机系统上预配和连接 Data Box Gateway 虚拟设备。 <br><br><br> 对于 VMware，可在运行 VMware ESXi 6.0、6.5 或 6.7 的主机系统上预配和连接 Data Box Gateway 虚拟设备。<br></br> |
| 3. |**[连接、设置并激活 Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md)** |连接到本地 Web UI，完成设备设置，然后激活设备。 然后即可预配 SMB 共享。  |
| 4. |**[使用 Data Box Gateway 传输数据](data-box-gateway-deploy-add-shares.md)** |添加共享，并通过 SMB 或 NFS 连接到共享。 |

现在可以开始设置 Azure 门户。

## <a name="prerequisites"></a>先决条件

本部分说明 Data Box Gateway 资源、Data Box Gateway 设备和数据中心网络的配置先决条件。

### <a name="for-the-data-box-gateway-resource"></a>对于 Data Box Gateway 资源

在开始之前，请确保：

* 应该为 Data Box Gateway 资源启用 Microsoft Azure 订阅。
* 具有 Microsoft Azure 存储帐户和访问凭据。

### <a name="for-the-data-box-gateway-device"></a>对于 Data Box Gateway 设备

在部署虚拟设备之前，请确保：

* 有权访问在 Windows Server 2012 R2 或更高版本上运行 Hyper-V 的主机系统或者运行可用于预配设备的 VMware（ESXi 6.0、6.5 或 6.7）的主机系统。
* 主机系统能够将以下资源专用于预配 Data Box 虚拟设备：
  
  * 至少 4 个核心。
  * 至少 8 GB 的 RAM。 
  * 一个网络接口。
  * 一个 250 GB 的 OS 磁盘。
  * 一个 2 TB 的用于系统数据的虚拟磁盘。

### <a name="for-the-datacenter-network"></a>对于数据中心网络

在开始之前，请确保：

* 数据中心内的网络按 Data Box Gateway 设备的网络要求配置。 有关详细信息，请参阅 [Data Box Gateway 系统要求](data-box-gateway-system-requirements.md)。

* Data Box Gateway 始终有专用的 20 Mbps Internet 带宽（或更高带宽）。 此带宽不应与任何其他应用程序共享。 如果使用网络限制，为使限制正常工作，我们建议使用 32 Mbps 或更高的 Internet 带宽。

## <a name="create-a-new-resource"></a>创建新资源

执行以下步骤，创建新的 Data Box Gateway 资源。 

如果现有的 Data Box Gateway 资源可以管理虚拟设备，请跳过此步骤并转到[获取激活密钥](#get-the-activation-key)。

在 Azure 门户中执行以下步骤以创建 Data Box 资源。
1. 使用 Microsoft Azure 凭据通过以下 URL 登录到 Azure 预览门户：[https://aka.ms/databox-edge](https://aka.ms/databox-edge)。 

2. 选择要用于 Data Box Edge 预览版的订阅。 选择要部署 Data Box Edge 资源的区域。 在“Data Box Gateway”选项中，单击“创建”。

    ![搜索 Data Box Gateway 服务](media/data-box-gateway-deploy-prep/data-box-gateway-edge-sku.png)

3. 对于新资源，请输入或选择以下信息。
    
    |设置  |值  |
    |---------|---------|
    |资源名称   | 用于标识资源的友好名称。<br>该名称的长度必须介于 2 和 50 个字符之间，只能包含字母、数字和连字符。<br> 名称以字母或数字开头和结尾。        |
    |订阅    |订阅将链接到你的计费帐户。 |
    |资源组  |选择现有的组，或创建新组。<br>详细了解 [Azure 资源组](../azure-resource-manager/resource-group-overview.md)。     |
    |位置     |此版本可在美国东部、美国西部 2 区、东南亚和西欧使用。 <br> 选择离要部署设备的地理区域最近的位置。|
    
    ![创建 Data Box Gateway 资源](media/data-box-gateway-deploy-prep/data-box-gateway-resource.png)
    
4. 单击“确定”。
 
创建资源需要几分钟时间。 成功创建资源后，你会收到相应的通知。


## <a name="download-the-virtual-device-image"></a>下载虚拟设备映像

创建 Data Box Gateway 资源后，下载相应的虚拟设备映像，以便在主机系统上预配虚拟设备。 虚拟设备映像特定于操作系统，可以在 Azure 门户中通过资源的“快速启动”边栏选项卡下载。

> [!IMPORTANT]
> 在 Data Box Gateway 上运行的软件只能结合 Data Box Gateway 资源使用。


在 [Azure 门户](https://portal.azure.com/)中执行以下步骤。

1. 单击创建的资源，然后单击“概述”。 如果有现有的 Azure Data Box Gateway 资源，请单击该资源并转到“概述”。

    ![新的 Data Box Gateway 资源](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

4. 在右窗格中的“快速启动”内，单击要下载的映像对应的链接。 映像文件约为 4.8 GB。
   
   * [Windows Server 2012 R2 及更高版本上的适用于 Hyper-V 的 VHDX](https://aka.ms/dbe-vhdx-2012)。
   * [适用于 VMWare ESXi 6.0、6.5 或 6.7 的 VMDK](https://aka.ms/dbe-vmdk)。

5. 下载该文件并将其解压缩到本地驱动器，记下解压缩文件所在的位置。


## <a name="get-the-activation-key"></a>获取激活密钥

Data Box Gateway 资源启动并运行后，需要获取激活密钥。 此密钥用于激活 Data Box Gateway 设备并将其连接到资源。

激活密钥用于将需要激活的所有 Data Box Gateway 设备注册到 Data Box Gateway 资源。 如果你仍在 Azure 门户中，则现在可以获取此密钥。

1. 单击创建的资源，然后单击“概述”。

    ![新的 Data Box Gateway 资源](media/data-box-gateway-deploy-prep/data-box-gateway-resource-created.png)

2. 单击“生成密钥”以创建激活密钥。 单击复制图标复制密钥，并将其保存供日后使用。

    ![获取激活密钥](media/data-box-gateway-deploy-prep/get-activation-key.png)

> [!IMPORTANT]
> - 生成的激活密钥将在 3 天后过期。 
> - 如果密钥已过期，请生成新密钥。 旧密钥不再有效。

## <a name="next-steps"></a>后续步骤

在本教程中，我们已了解有关 Data Box Gateway 的主题，例如：

> [!div class="checklist"]
> * 创建新资源
> * 下载虚拟设备映像
> * 获取激活密钥

请继续学习下一篇教程，了解如何为 Data Box Gateway 预配虚拟机。 根据主机操作系统参阅以下操作的详细说明：

> [!div class="nextstepaction"]
> [在 Hyper-V 中预配 Data Box Gateway](./data-box-gateway-deploy-provision-hyperv.md)

或

> [!div class="nextstepaction"]
> [在 VMware 中预配 Data Box Gateway](./data-box-gateway-deploy-provision-vmware.md)


