---
title: 快速入门 - 通过成本分析了解 Azure 成本 | Microsoft Docs
description: 本快速入门可帮助你通过成本分析了解和分析 Azure 组织成本。
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 01/07/2019
ms.topic: quickstart
ms.service: cost-management
manager: dougeby
ms.custom: seodec18
ms.openlocfilehash: cb07ce71162a766add5ca251c97a11d353ee8084
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54077651"
---
# <a name="quickstart-explore-and-analyze-costs-with-cost-analysis"></a>快速入门：通过成本分析了解和分析成本

需要知道你组织中哪些方面产生了成本，然后才能正确地控制和优化 Azure 成本。 这还有助于了解你服务的成本及其支持哪些环境和系统。 要准确地了解组织的支出模式，必须了解成本的方方面面。 在强制实施成本控制机制（如预算）时可使用支出模式。

在本快速入门中，你将通过成本分析了解和分析组织成本。 可以按组织查看合计成本，了解哪些方面持续产生成本并确定支出趋势。 可以查看一段时间的累计成本，根据预算预估每月、每季度甚至每年的成本趋势。 预算可帮助你遵守财务约束。 并且预算用于查看每日或每月成本，以隔离异常支出行为。 你还可下载当前报表的数据进行深入分析或在外部系统中使用。

此快速入门介绍如何：

- 通过成本分析查看成本
- 自定义成本视图
- 下载成本分析数据


## <a name="prerequisites"></a>先决条件

所有[企业协议 (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) 客户均可使用成本分析。 必须至少具有以下一个或多个范围的读取权限才能查看成本数据。 若要详细了解如何分配对成本管理数据的访问权限，请参阅[分配对数据的访问权限](assign-access-acm-data.md)。

- 计费帐户
- 部门
- 注册帐户
- 管理组
- 订阅
- 资源组

## <a name="sign-in-to-azure"></a>登录 Azure

- 通过 https://portal.azure.com 登录到 Azure 门户。

## <a name="review-costs-in-cost-analysis"></a>通过成本分析查看成本

若要通过成本分析查看成本，请在 Azure 门户中导航至“成本管理 + 计费”&gt;“成本管理”&gt;“更改范围”，选择范围，然后单击“选择”。

所选的范围将用于整个成本管理，以提供数据整合和控制对成本信息的访问。 使用范围时，不要多选它们。 而应选择一个其他范围汇总到的较大范围，然后筛选到所需的范围。 理解这一点很重要，因为有些人不应该有权访问子范围汇总到的父范围。

单击“打开成本分析”。

初始成本分析视图包括以下方面：

总计 - 显示当月的总成本。

预算 - 显示所选范围的计划支出限额（如可用）。

累计成本 - 显示从本月初算起的累计每日支出总额。 为计费帐户或订阅[创建预算](tutorial-acm-create-budgets.md)后，可快速查看对预算而言的支出趋势。 将鼠标悬停某个日期上以查看该天的累计成本。

透视图（圆环图）– 提供动态透视，按一组常见标准属性分解总成本。 它们显示当月累计的最高成本至最低成本。 通过选择不同的透视，可以随时更改透视图。 成本按以下类别分类：服务（计量类别）、位置（区域）和子范围（默认）。 例如，注册帐户在计费帐户之下，资源组在订阅之下，资源在资源组之下。

![Azure 门户中成本分析的初始视图](./media/quick-acm-cost-analysis/cost-analysis-01.png)

## <a name="customize-cost-views"></a>自定义成本视图

通过默认视图，可快速解答常见问题，例如：

- 我支出了多少？
- 在预算范围内吗？

但是，在很多情况下需要更深入的分析。 在页面顶部通过数据选择进行自定义。

默认情况下，成本分析显示当前月份的数据。 使用日期选择器可快速切换到：上个月、本月、本季度、本年度或所选的自定义日期范围。 要分析最近的 Azure 发票并轻松进行对账，最快速的方式是选择上个月。 当前季度和年度选项有助于跟踪针长期预算的成本。 也可以选择不同的日期范围。 例如，可以选择一天、过去七天或当前月之前一年的任何时间。

![显示本月示例选择的日期选择器](./media/quick-acm-cost-analysis/date-selector.png)

默认情况下，成本分析显示“累计”成本。 累计成本显示包括前几天在内的每日费用总额，显示每日应计成本不断变化的情况。 此视图已进行优化，可显示对预算而言所选日期范围的支出趋势。

此外，还有每日视图显示每日成本。 每日视图不会显示增长趋势。 该视图旨在显示异常，如每日的成本峰值或 dip。 如果选择了预算，则每日视图还会显示每日预算的估算值。 如果你的每日成本不断高于估算的每日预算，则可能会超出每月预算。 估算的每日预算只是一种可在较浅显的层面上帮助你直观查看预算的方式。 如果每日成本出现波动，则与每月预算比较，估算的每日预算不太准确。

通常情况下，有望在 8 小时内看到所用资源的数据或者通知。

![显示当月的示例每日成本的每日视图](./media/quick-acm-cost-analysis/daily-view.png)

可通过“分组依据”选择分组类别，从而更改顶部“总计”面积图中显示的数据。 通过分组，可以快速查看支出是如何按常用资源和使用属性（例如资源组或资源标记）分类的。 若要按标记分组，请选择要按其分组的标记键。 然后会看到成本按该标记的每个值进行细分，并且有一个额外的段，用于未应用该标记的资源。

大多数 [Azure 资源支持标记](../azure-resource-manager/tag-support.md)，但某些标记在“成本管理”和“计费”中不可用。 此外，不支持资源组标记。 成本管理仅自标记直接应用到资源之日起支持资源标记。

下面是 Azure 服务成本的上月视图。

![显示上个月示例 Azure 服务成本的分组的每日汇总视图](./media/quick-acm-cost-analysis/grouped-daily-accum-view.png)

主图下的透视图显示了不同的分组，让你在更大范围内了解所选时段和筛选器对应的总体成本。 选择一个属性或标记即可按任意维度查看聚合的成本。 展开“数据”抽屉或选择屏幕顶部的“导出”**>**“下载 CSV”即可找到屏幕底部的总视图的完整数据集。 下面是资源组的数据抽屉的示例。

![显示资源组名称的当前视图的完整数据](./media/quick-acm-cost-analysis/full-data-set.png)

上图显示了资源组名称。 虽然可以按标记分组，以便按标记查看总成本，但在任何成本分析视图中，都不可按资源或资源组来查看所有标记。

当按特定的属性对成本进行分组时，将按从最高到最低的顺序显示排名前十的成本贡献因素。 如果有 10 个以上的组，则会显示前九个成本贡献因素。 同时显示的是**其他**组，它将所有剩余的组一起涵盖。 按标记分组时，也可能会看到成本的“非标记”组，这是一个未应用标记键的组。 **非标记**总是位于最后，即使非标记成本的数目多于标记成本的数目。 如果有 10 个或更多的标记值，则非标记成本将会列在“其他”中。

“经典”（Azure 服务管理，简称 ASM）虚拟机、网络和存储资源不共享详细的计费数据。 当对成本进行分组时，它们合并为**经典服务**。


## <a name="download-cost-analysis-data"></a>下载成本分析数据

可以从“成本分析”下载信息，为 Azure 门户中当前显示的所有数据生成一个 CSV 文件。 应用的任何筛选或分组均包含在文件中。 顶部“总计”视图中灰显的基础数据也会包含在 CSV 文件中。

## <a name="next-steps"></a>后续步骤

转到第一个教程，了解如何创建并管理预算。

> [!div class="nextstepaction"]
> [创建并管理预算](tutorial-acm-create-budgets.md)
