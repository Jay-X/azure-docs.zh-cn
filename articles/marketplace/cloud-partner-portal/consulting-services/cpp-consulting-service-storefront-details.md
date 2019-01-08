---
title: Azure 和 Dynamcis 365 咨询服务套餐 - 输入店面详细信息 | Microsoft Docs
description: 有关在云合作伙伴门户中为 Azure 或 Dynamics 365 咨询服务套餐定义店面详细信息的指导。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: pbutlerm
ms.openlocfilehash: f587ca92c83680526a277a571eea98e73b82d902
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/13/2018
ms.locfileid: "53344978"
---
# <a name="storefront-details-tab"></a>“店面详细信息”选项卡

接下来，需要输入店面的详细信息。 “店面详细信息”由以下部分组成：

-   套餐详细信息
-   发布者信息
-   商品详细信息
-   营销项目

![创建新的咨询服务套餐 -“店面详细信息”选项卡](media/consultingoffer-storefront-details.png)

## <a name="offer-details"></a>套餐详细信息

套餐详细信息部分包含以下字段：

-   套餐摘要
-   套餐说明

### <a name="offer-summary"></a>套餐摘要

套餐摘要是套餐的简要说明，紧靠在套餐名称的下面显示。 输入套餐摘要时请使用纯文本，且不要换行。 下面是适当的套餐摘要及其相应套餐名称的示例：

*示例 1*

-   **套餐名称：** 云分析：3 日研讨会
-   **套餐摘要：** Microsoft Azure 和 Power BI、当前环境评估与微型 POC 的概述。

*示例 2*

-   **套餐名称：** 行业 Azure IoT：30 日概念证明
-   **套餐摘要：** 创建行业联网试点产品，以便将现场设备安全连接到提供仪表板、报告和通知的 Azure IoT 中心解决方案。

*示例 3*

-   **套餐名称：** 专业服务：1 小时简报
-   **套餐摘要：** 预配置扩展型 Dynamics 365 运营版解决方案的概述和演示，此版本可为专业服务提供增强的项目、计费和资源管理。

*示例 4*

-   **套餐名称：** Your World 中的 Power BI：4 小时研讨会
-   **套餐摘要：** 开始运行第一个仪表板并了解最佳做法。 现场举行，参与者不超过 12 名学生。

*示例 5*

-   **套餐名称：** Dynamics 和项目：3 日评估
-   **套餐摘要：** 针对 ERP 解决方案的需求收集与评估，该解决方案专为专业服务公司和项目驱动型企业而设计。

### <a name="offer-description"></a>套餐说明

在此处输入咨询服务套餐的说明。 合理的套餐说明包括互动功能的具体详细信息，以及最终向客户交付的内容。 此说明应该可以帮助客户明确了解他们会得到的服务。 套餐说明需要介绍该套餐与你要为其提供咨询服务的 Microsoft 产品的关系。

不要在套餐说明中包含电子邮件链接或联系电话号码。 套餐中应该附带一个“与我联系”按钮，使用该按钮可将潜在顾客上传到你在套餐中指定的潜在顾客管理目标。

以 Markdown 格式输入套餐说明。 如果不熟悉 Markdown 或 HTML 格式，可以查看[如何使用 Markdown 撰写 Docs](https://docs.microsoft.com/contribute/how-to-write-use-markdown) 上的资源。

使用这些格式可确保客户能够最方便地阅读你的套餐内容。

套餐说明应该简单扼要，并遵守字符数限制，因为用户并不乐意阅读很长的文字。 但仍可以选择上传更详细地描述套餐的营销手册、事实表和其他文档。

以下示例演示了一段适当编写的套餐说明及其相关名称和摘要：

**套餐名称：** 云分析：3 日研讨会

**套餐摘要：** Microsoft Azure 和 Power BI、当前环境评估与微型 POC 的概述。

**套餐说明：** 这场为期 3 天的研讨会面向技术和业务主管，在客户现场举行。

***议程***

第 1 天

-   重点探讨如何使用 Azure Data Lake、HDInsight 或 Azure SQL 数据仓库在 Microsoft 云中保护、缩放和组织数据。

第 2 天

-   探讨如何使用 Microsoft R 和 Azure 机器学习来配置和部署高级分析解决方案。

第 3 天

-   探讨如何使用 Power BI 来提取可行见解并将分析结果操作化，包括举行一场协作短会来共同生成 Power BI 仪表板。

此次研讨会结束时，客户可为 Microsoft 云中的数据和分析解决方案定义高级计划和实施路线图。

*遵循此格式的套餐示例 Markdown 文件：*

    This 3-day workshop is for technical and business leaders and is held on-site at the client’s facility.

      ### Agenda

      **Day 1**

      * Focuses on how to secure, scale, and organize data within the Microsoft cloud using Azure Data Lake, HDInsight, or Azure SQL Data Warehouse

      **Day 2**

      * Covers how to configure and deploy advanced analytics solutions with Microsoft R and Azure Machine Learning

      **Day 3**

      * Covers how to draw actionable insights and operationalize analytics with Power BI, including a collaborative session to co-build a Power BI dashboard.


      ### Deliverables
      By the end of the workshop, the client will be able to define a high-level plan and an implementation roadmap for data and analytics solutions in the Microsoft cloud.


## <a name="publisher-information"></a>发布者信息

**MPN ID**

9 位数 Microsoft 合作伙伴网络 (MPN) ID。 如果没有 MPN ID，可以转到 Microsoft 合作伙伴中心获取一个 ID。

**合作伙伴中心 ID**

新的合作伙伴中心 ID（如果有）。

**MPN ID**

输入机密密钥，以便在套餐上线之前，在 AppSource 中对其进行预览。
此标识符并非密码。

## <a name="listing-details"></a>商品详细信息

**咨询服务类型**

Microsoft 专门注重提供给单个客户的固定范围、固定持续时间、估算价格、固定价格（或免费），以及主要面向售前的咨询服务套餐，另外还关注在现场或以虚拟形式开展的评估、实施、概念证明、简报或研讨会套餐。 AppSource 咨询服务市场不支持适用于托管服务或订阅服务的商品。

>[!Note]
>AppSource 咨询服务不是订阅或按需训练的适当市场。

包括以下五种类型的套餐：

-   **评估：** 评估客户的环境，以确定解决方案的适用性，并提供成本和时间估算。
-   **简报：** 使用框架、演示和客户示例介绍解决方案或咨询服务，以吸引客户的兴趣。 简报必须在现场开展。
-   **实施：** 进行完整安装，以实施完全正常运行的解决方案。 对于此试点套餐，Microsoft 建议限制为可在一周或更短时间内实施的解决方案。
-   **概念证明：** 进行有限范围的实施，以确定解决方案是否满足客户的要求。
-   **研讨会：** 在客户现场开展的互动活动，可包括培训会议、简报、评估，或根据客户数据或环境制作的演示。

**国家/区域可用性**

选择提供此咨询服务套餐的国家和地区。 单个套餐不能在多个国家/地区发布。 必须为每个国家或地区创建一个新套餐。

>[!Note]
>AppSource 咨询服务目前在美国、英国和加拿大开展。 可以为某个国家/地区提交一项尚未提供的套餐，该套餐在经过审核后就可以提供。 若要开拓新的国家/地区，至少需要提供一定数量的套餐，因此我们鼓励用户为此类国家/地区提供套餐。

**行业**

选择咨询服务套餐最适合的行业。

**持续时间**

在“持续时间”下选择一个数字（例如 3、4，等等），然后选择“小时”、“天”或“周”。

**主要产品**

若要发布到 Azure 市场，请选择“Azure”作为主要产品，然后选择相关的“解决方案区域”。

若要发布到 AppSource，请选择“Dynamics 365”、“Power BI”或“PowerApps”作为主要产品。 还可以选择其他相关的“适用产品”，你的咨询服务套餐将显示在 AppSource 上与其中每个产品关联的商品清单中。

**相关资质**

选择与此套餐相关的资质，以便与套餐详细信息一同显示。

## <a name="marketing-artifacts"></a>营销项目

**公司徽标（.png 格式，48 x 48 像素）**

上传要显示在套餐库视图页中套餐磁贴上的图像。 该图像必须是 .png 格式，分辨率为 48 x 48 像素。

**公司徽标（.png 格式，216 x 216 像素）**

上传要显示在套餐详细信息页上的图像。 该图像必须是 .png 格式，分辨率为 216 x 216 像素。

**视频（最多 4 个）**

最多上传四个客户案例研究视频或客户参考视频。 如果没有此类视频，请上传一个视频来介绍贵公司在套餐方面的专业技术。 如果有 Power BI 或 PowerApps 解决方案的展示，请在此处上传展示视频。 视频必须链接到 YouTube 或 Vimeo。

**文档（最多 3 个）**

上传详细描述咨询服务套餐的营销手册。 或者，也可以上传公司概况、事实表或案例研究。 请确保文档使用特色产品的当前名称，并且不要推荐与 Microsoft 竞争的产品。

**屏幕截图（最多 5 个）**

最多上传五个图像，以提供有关套餐、套餐交付件或公司的详细信息。 例如，营销手册的片段、演示文稿中的相关幻灯片，或者展示公司发展势头或专业知识的图像。

## <a name="next-steps"></a>后续步骤

现在可以[发布咨询服务](./cpp-consulting-service-publish-offer.md)套餐了。