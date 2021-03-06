---
title: Azure 媒体服务中的流式处理策略 | Microsoft Docs
description: 本文介绍何为流式处理策略以及 Azure 媒体服务如何使用这些终结点。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/22/2018
ms.author: juliako
ms.openlocfilehash: d74ce913a2189dd1062b30f9def919cbbabe7b64
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742518"
---
# <a name="streaming-policies"></a>流式处理策略

在 Azure 媒体服务 v3 中，使用流式处理策略，可为 StreamingLocators 定义流协议和加密选项。 可以指定创建的流式处理策略的名称，或使用一个预定义流式处理策略。 目前可用的预定义流式处理策略包括：“Predefined_DownloadOnly”、“Predefined_ClearStreamingOnly”、“Predefined_DownloadAndClearStreaming”、“Predefined_ClearKey”、“Predefined_MultiDrmCencStreaming”和“Predefined_MultiDrmStreaming”。

> [!IMPORTANT]
> 使用自定义的 [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies) 时，应为媒体服务帐户设计有限的一组此类策略，并在需要同样的加密选项和协议时重新将这些策略用于流式处理定位符。 媒体服务帐户具有对应于流式处理策略条目数的配额。 不应为每个流式处理定位符创建新的流式处理策略。

## <a name="streamingpolicy-definition"></a>StreamingPolicy 定义

下表显示了 StreamingPolicy 的属性并给出了它们的定义。

|名称|Description|
|---|---|
|id|资源的完全限定的资源 ID。|
|名称|资源的名称。|
|properties.commonEncryptionCbcs|CommonEncryptionCbcs 的配置|
|properties.commonEncryptionCenc|CommonEncryptionCenc 的配置|
|properties.created |流式处理策略的创建时间|
|properties.defaultContentKeyPolicyName |当前流式处理策略使用的默认 ContentKey|
|properties.envelopeEncryption  |EnvelopeEncryption 的配置|
|properties.noEncryption|NoEncryption 的配置|
|type|资源的类型。|

有关完整定义，请参阅[流式处理策略](https://docs.microsoft.com/rest/api/media/streamingpolicies)。

## <a name="filtering-ordering-paging"></a>筛选、排序、分页

媒体服务支持对流式处理策略使用以下 OData 查询选项： 

* $filter 
* $orderby 
* $top 
* $skiptoken 

运算符说明：

* Eq = 等于
* Ne = 不等于
* Ge = 大于或等于
* Le = 小于或等于
* Gt = 大于
* Lt = 小于

### <a name="filteringordering"></a>筛选/排序

下表显示了可以如何将这些选项应用于 StreamingPolicy 属性： 

|名称|筛选器|顺序|
|---|---|---|
|id|||
|名称|Eq、ne、ge、le、gt、lt|升序和降序|
|properties.commonEncryptionCbcs|||
|properties.commonEncryptionCenc|||
|properties.created |Eq、ne、ge、le、gt、lt|升序和降序|
|properties.defaultContentKeyPolicyName |||
|properties.envelopeEncryption|||
|properties.noEncryption|||
|type|||

### <a name="pagination"></a>分页

已启用的四个排序顺序均支持分页。 当前，页面大小为 10。

> [!TIP]
> 应始终使用下一个链接来枚举集合，而不依赖特定的页面大小。

如果查询响应包含许多项，则服务将返回一个“\@odata.nextLink”属性来获取下一页结果。 这可用于逐页浏览整个结果集。 无法配置页面大小。 

如果在逐页浏览集合时创建或删除 StreamingPolicy，则会在返回的结果中反映此更改（如果这些更改位于集合中尚未下载的部分）。 

以下 C# 示例显示如何枚举帐户中的所有 StreamingPolicies。

```csharp
var firstPage = await MediaServicesArmClient.StreamingPolicies.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.StreamingPolicies.ListNextAsync(currentPage.NextPageLink);
}
```

有关 REST 示例，请参阅[流式处理策略 - 列表](https://docs.microsoft.com/rest/api/media/streamingpolicies/list)

## <a name="next-steps"></a>后续步骤

[流式传输文件](stream-files-dotnet-quickstart.md)
