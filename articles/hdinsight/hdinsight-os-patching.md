---
title: 为基于 Linux 的 HDInsight 群集配置 OS 修补计划 - Azure
description: 了解如何为基于 Linux 的 HDInsight 群集配置 OS 修补计划。
services: hdinsight
author: omidm1
ms.author: omidm
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/24/2019
ms.openlocfilehash: 402a4d59b57803b8a9c0094799ceee6a92df43f9
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911347"
---
# <a name="os-patching-for-hdinsight"></a>针对 HDInsight 的 OS 修补 

> [!IMPORTANT]
> Ubuntu 映像可在发布后的 3 个月内用于创建新的 HDInsight 群集。 自 2019 年 1 月起，系统**不**会自动修补正在运行的群集。 客户必须使用脚本操作或其他机制来修补正在运行的群集。

## <a name="how-to-configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>如何为基于 Linux 的 HDInsight 群集配置 OS 修补计划
需不定期重启 HDInsight 群集中的虚拟机，以便安装重要的安全修补程序。 自 2016 年 8 月 1 日起，已使用以下计划重启新的基于 Linux 的 HDInsight 群集（版本 3.4 或更高版本）：

1. 群集中的虚拟机最多只能在 30 天内重启一次以进行修补。
2. 重启从晚上 12 点 (UTC) 开始。
3. 重启过程在群集中的虚拟机之间交错进行，因此群集在重启过程中仍然可用。
4. 新创建的群集不会在创建日期后的 30 天内发生第一次重启。

使用本文中描述的脚本操作，可以按如下所示修改 OS 修补计划：
1. 启用或禁用自动重启
2. 设置重启频率（重启之间的天数）
3. 设置在星期几进行重启

> [!NOTE]  
> 此脚本操作仅适用于 2016 年 8 月 1 日后创建的基于 Linux 的 HDInsight 群集。 仅在重启 VM 后，修补程序才生效。 

## <a name="how-to-use-the-script"></a>如何使用脚本 

使用此脚本需要以下信息：
1. 脚本位置： https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv01/os-patching-reboot-config.sh。HDInsight 使用此 URI 在群集中的所有虚拟机上查找并运行脚本。
  
2. 应用该脚本的群集节点类型：头节点、辅助节点、zookeeper。 此脚本必须应用于群集中的所有节点类型。 如果不将其应用于某个节点类型，则该节点类型的虚拟机将继续使用以前的修补计划。


3.  参数：此脚本接受三个数字参数：

    | 参数 | 定义 |
    | --- | --- |
    | 启用/禁用自动重启 |0 或 1。 值为 0 表示禁用自动重启，而值为 1 则表示启用自动重启。 |
    | 频率 |7 到 90（含）。 在为需要重启才能生效的修补程序重启虚拟机之前等待的天数。 |
    | 星期几 |1 到 7（含）。 值为 1 表示在星期一进行重启，值为 7 表示在星期日进行重启。例如，使用参数 1 60 2，则其结果为虚拟机每隔 60 天（最多）在星期二自动重启。 |
    | 持久性 |将脚本操作应用于现有群集时，可将该脚本标记为持久化。 通过缩放操作将新的辅助节点添加到群集时，应用持久化脚本。 |

> [!NOTE]  
> 将其应用于现有群集时，必须将此脚本标记为持久化。 否则，通过缩放操作创建的任何新节点都将使用默认修补计划。  如果在群集创建过程中应用该脚本，则其会自动持久化。

## <a name="next-steps"></a>后续步骤

若要了解使用脚本操作的具体步骤，请参阅[使用脚本操作自定义基于 Linux 的 HDInsight 群集](hdinsight-hadoop-customize-cluster-linux.md)中的以下部分：

* [在创建群集期间使用脚本操作](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [将脚本操作应用到正在运行的群集](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
