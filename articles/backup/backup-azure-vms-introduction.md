---
title: 关于 Azure VM 备份
description: 了解 Azure VM 备份，并记下一些最佳做法。
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: raynew
ms.openlocfilehash: 57d52412648cbe8a0791aa306075018a2092bf51
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827323"
---
# <a name="about-azure-vm-backup"></a>关于 Azure VM 备份

本文介绍 [Azure 备份服务](backup-introduction-to-azure-backup.md)如何备份 Azure VM。

## <a name="backup-process"></a>备份过程

以下演示 Azure 备份如何为 Azure VM 完成一次备份。

1. 选定要备份的 Azure VM 后，Azure 备份服务会根据指定的备份计划启动备份作业。
2. 该服务会触发备份扩展。
    - Windows VM 使用 _VMSnapshot_ 扩展。
    - Linux VM 使用 _VMSnapshot_ 扩展。
    - 在第一个 VM 备份期间安装扩展。
    - 若要安装扩展，VM 必须处于运行状态。
    - 如果 VM 未运行，备份服务将创建基础存储的快照（因为在 VM 停止时不会发生任何应用程序写入）。
4. 备份扩展会采用存储级别的、故障一致性/文件一致性快照。
5. 生成快照后，数据传输到保管库中。 为最大限度地提高效率，服务仅标识和传输自上次备份（增量）以后已更改的数据块。
5. 数据传输完成后，会删除快照并创建恢复点。

![Azure 虚拟机备份体系结构](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

## <a name="data-encryption"></a>数据加密

在备份过程中，Azure 备份不会加密数据。 Azure 备份支持为通过 Azure 磁盘加密进行了加密的 Azure VM 进行备份。

- 对于托管和非托管 Azure VM，支持仅经过 BitLocker 加密密钥 (BEK) 加密的以及同时经过 BEK 和密钥加密密钥 (KEK) 加密的 VM 备份。
- BEK（机密）和 KEK（密钥）备份已经过加密，以确保它们只能在还原到密匙保管库后由授权用户读取和使用。
- 由于同时备份了 BEK，因此在 BEK 丢失的情况下，授权用户可以将 BEK 还原到密匙保管库并恢复加密的 VM。 加密后的 VM 的密钥和机密以加密形式备份，因此未经授权的用户以及 Azure 都无法读取或使用备份密钥和机密。 只有具有正确的权限级别的用户才可备份和还原加密的 VM 以及密钥和机密。

## <a name="snapshot-consistency"></a>快照一致性

为了在应用运行过程中拍摄快照，Azure 备份会拍摄应用一致的快照。

- **Windows VM**：对于 Windows VM，备份服务与卷影复制服务 (VSS) 互相配合，来获取 VM 磁盘的一致性快照。
    - 默认情况下，Azure 备份会获取完整的 VSS 备份。 [了解详细信息](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)。
    - 如要更改设置，使 Azure 备份获取 VSS 副本备份，请设置以下注册表项：
        ```
        [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
        ""USEVSSCOPYBACKUP"="TRUE"
        ```
        - 从提升（作为管理员）命令提示符运行以下命令，以设置上述注册表项：
          ```
          REG ADD "HKLM\SOFTWARE\Microsoft\BcdrAgent" /v USEVSSCOPYBACKUP /t REG_SZ /d TRUE /f
          ```
- **Linux VM**：若要在 Azure 备份拍摄快照时确保 Linux VM 是应用程序一致的，可以使用 Linux 前脚本和后脚本框架。 可以编写自定义脚本来确保创建 VM 快照时的一致性。
    -  Azure 备份只调用你编写的操作前脚本和操作后脚本。
    - 如果操作前脚本和操作后脚本成功执行，Azure 备份会将恢复点标记为应用程序一致。 但是，你最终为使用自定义脚本时的应用程序一致性负责。
    - [了解](backup-azure-linux-app-consistent.md)有关配置脚本的详细信息。


#### <a name="consistency-types"></a>一致性类型

下表提供了不同类型的一致性的相关说明。

**快照** | **详细信息** | **恢复** | **注意事项**
--- | --- | --- | ---
**应用程序一致** | 应用一致性备份捕获内存内容和挂起的 I/O 操作。 应用一致性快照使用 VSS 编写器（或适用于 Linux 的前期/后期脚本），可确保备份发生之前应用数据的一致性。 | 使用应用一致性快照恢复时，会启动 VM。 没有数据损坏或丢失。 应用以一致的状态启动。 | Windows:所有 VSS 编写器均成功<br/><br/> Linux：前/后脚本已配置并成功
**文件系统一致** | 文件一致性备份通过同时拍摄所有文件的快照来提供一致的磁盘文件备份。<br/><br/> | 使用文件一致快照进行恢复时，会启动 VM。 没有数据损坏或丢失。 应用需要实现自己的“修复”机制以确保还原的数据一致。 | Windows:部分 VSS 编写器失败 <br/><br/> Linux：默认值（如果前/后脚本未配置或失败）
**故障一致** | Azure VM 在备份期间关闭时经常会出现崩溃一致性问题。  仅会捕获和备份备份时磁盘上已存在的数据。<br/><br/> 存在崩溃一致性问题的恢复点不能保证操作系统或应用的数据一致性。 | 在无法保证一致性的情况下，VM 通常仍会启动，随后执行磁盘检查，试图修复崩溃错误。 任何未传输到磁盘的内存中数据或写入数据将丢失。 应用实现自己的数据验证。 例如，对于数据库应用，如果事务日志中有条目不在数据库中，则数据库软件将执行滚动，直到数据一致。 | VM 处于关闭状态


## <a name="service-and-subscription-limits"></a>服务和订阅限制

Azure 备份存在许多关于订阅和保管库的限制。

[!INCLUDE [azure-backup-limits](../../includes/azure-backup-limits.md)]


## <a name="backup-performance"></a>备份性能

### <a name="disk-considerations"></a>磁盘注意事项

通过并行备份每个 VM 磁盘来优化备份操作。 例如，如果 VM 有 4 个磁盘，则服务会尝试并行备份所有 4 个磁盘。 对于每个要备份的磁盘，Azure 备份将读取磁盘上的块，并只存储更改的数据（增量备份）。


### <a name="scheduling-considerations"></a>计划注意事项

备份计划会对性能产生影响。

- 如果将策略配置为同时备份所有 VM，那就和计划了一次“数据塞车”没区别，因为备份过程会尝试并行备份所有磁盘。
- 若要减少备份流量，请在一天的不同时间备份不同 VM，并且时间不重叠。


### <a name="time-considerations"></a>时间注意事项

尽管大部分备份时间花在读取和复制数据上，但其他一些操作也会增加到备份 VM 所需的总时间：

- **安装备份扩展名**：花费在安装或更新备份扩展上的时间。
- **触发快照**：触发快照所需的时间。 在接近计划的备份时间时，会触发快照。
- **队列等待时间**：由于备份服务同时处理来自多个客户存储账户的作业，快照数据可能无法立即复制到恢复服务保管库。 在负载高峰，最长可能需要八小时才能处理完备份。 但是，每日备份策略规定的 VM 备份总时间不会超过 24 小时。
- **初始备份**：尽管总备份时间不超过 24 小时对增量备份有效，但可能不适用于第一个备份。 所需的时间取决于数据的大小和备份执行时的时间。
- **数据传输时间**：备份服务所需的时间计算上一备份中的增量更改并将更改传输到保管库存储所需的时间。

### <a name="factors-affecting-backup-time"></a>影响备份时间的因素

备份包含两个阶段，获取快照和将快照传输到保管库。 备份服务针对存储进行相关优化。

- 将快照数据传输到保管库时，服务仅传输上一个快照的增量更改。
- 为确定增量更改，服务会计算块的校验和。
    - 如果一个块发生更改，则该块会被标识为要发送到保管库的块。
    - 服务进一步钻取到每个已标识块，寻找机会尽量减少要传输的数据。
    - 评估所有已更改块后，服务将联合更改并将其发送到保管库。

以下情况可能会影响备份时间：

- **新添加到已受保护 VM 的磁盘的初始备份**：如果 VM 正在进行增量备份，并且新磁盘已添加到此 VM，则备份持续时间可能超过 24 小时，因为新添加的磁盘必须进行初始复制以及现有磁盘的增量复制。
- **碎片**：备份产品会扫描两个备份操作之间的增量更改。 与磁盘上的更改分布相比，当磁盘上的更改并置时，备份操作会更快。 
- **变动**：超过 200 GB 的磁盘的每日变动频率（用于增量复制）可能需要大于 8 小时才能完成该操作。 如果 VM 有多个磁盘，并且其中某个磁盘需要较长的备份时间，则可能会影响整个备份操作（或者可能导致失败）。 
- **校验和比较 (CC) 模式**：CC 模式比即时 RP 使用的优化模式慢。 如果已使用即时 RP 并删除了第 1 层快照，则备份切换到 CC 模式会导致备份操作超过 24 小时（或失败）。

## <a name="restore-considerations"></a>还原注意事项

还原操作包括两个主要任务：将数据从保管库复制回所选的存储帐户和创建虚拟机。 从保管库中复制数据所需的时间取决于备份在 Azure 中存储的位置以及存储帐户的位置。 复制数据所花的时间取决于：

- **队列等待时间**：由于服务同时处理来自多个存储账户的还原作业，因此还原请求会排入队列中。
- **数据复制时间**：将数据从保管库复制到存储帐户。 还原时间取决于 Azure 备份服务所使用的所选存储帐户的 IOPS 和吞吐量。 若要减少还原过程期间的复制时间，请选择一个未加载其他应用程序写入和读取的存储帐户。

## <a name="best-practices"></a>最佳做法

建议在为 VM 配置备份时遵循以下做法：

- 请考虑修改默认提供的策略时间（例如， 如果默认策略时间是上午 12:00，请考虑将其递增几分钟），同时采用数据快照以确保最佳地使用资源。
- 对于非即时 RP 功能上的高级 VM 备份，分配约 50% 的总存储帐户空间。 备份服务需要此空间，以将快照复制到同一存储帐户并将其传输到保管库。
- 要从单个保管库还原 VM，强烈建议使用不同的  [v2 存储帐户](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) ，以确保目标存储帐户不会受到限制。 例如，每个 VM 必须具有不同的存储帐户（如果还原 10 个 VM，请考虑使用 10 个不同的存储帐户）。
- 第 1 层存储层（快照）的还原将在几分钟内完成（因为它是相同的存储帐户），而第 2 层存储层（保管库）可能需要数小时。 针对数据在第 1 层中可用的情况，建议使用[即时还原](backup-instant-restore-capability.md)功能以便更快地还原（如果必须从保管库还原数据，则需要时间）。
- 每个存储帐户的磁盘数限制与 IaaS VM 上运行的应用程序访问磁盘的大小有关。 请验证单个存储帐户是否托管了多个磁盘。 通常情况下，如果单个存储帐户上存在 5 至 10 个或以上磁盘，则通过将一些磁盘移动到单独的存储帐户以均衡负载。

## <a name="backup-costs"></a>备份成本

使用 Azure 备份备份的 Azure VM 采用 [Azure 备份定价](https://azure.microsoft.com/pricing/details/backup/)。

- 第一个备份成功完成后才会开始计费。 存储和受保护的实例也会在此同时开始计费。
- 只要针对虚拟机的任何备份数据存储在保管库中就会持续计费。 如果在虚拟机上停止保护，但在保管库中仍然存在虚拟机备份数据，仍将继续计费。
- 针对特定 VM 的计费仅在停止保护并且删除全部备份数据后才会停止。
- 当停止保护并且没有活动的备份作业时，最后一个成功的 VM 备份的大小将成为用于每月帐单的受保护实例大小。
- 受保护的实例计算基于虚拟机的实际大小，即虚拟机中除临时存储外的所有数据之和。
- 定价基于数据磁盘中存储的实际数据。
- VM 备份定价并非基于附加到虚拟机的每个数据磁盘的最大支持大小。
- 与此类似，备份存储的收费是基于 Azure 备份中存储的数据量，即每个恢复点中实际数据之和。

以 A2 标准大小的虚拟机为例，该虚拟机有两个额外的数据磁盘，总大小为 4 TB。 下表提供了其中每个磁盘上存储的实际数据：

| 磁盘类型 | 最大大小 | 实际存在的数据 |
| --------- | -------- | ----------- |
| 操作系统磁盘 |4095 GB |17 GB |
| 本地磁盘/临时磁盘 |135 GB |5 GB（不包括在备份中） |
| 数据磁盘 1 |4095 GB |30 GB |
| 数据磁盘 2 |4095 GB |0 GB |

- 此示例中，VM 虚拟机的实际大小为 17 GB + 30 GB + 0 GB = 47 GB。
- 此受保护实例大小 (47 GB) 成为按月计费的基础。
- 随着虚拟机中数据量的增长，用于计费的受保护实例大小也会相应变化。


## <a name="next-steps"></a>后续步骤

了解了备份过程和性能注意事项后，请执行以下步骤：

- [了解](../virtual-machines/windows/premium-storage-performance.md)如何优化应用以获取 Azure 存储的最佳性能。 本文内容主要探讨高级存储，但同样适用于标准存储磁盘。
- 通过了解 VM 支持和限制、创建保管库以及准备好备份用 VM，[开始使用](backup-azure-arm-vms-prepare.md)备份功能。
