---
title: 所有预构建实体
titleSuffix: Azure Cognitive Services
description: 本文包含了语言理解 (LUIS) 中包括的预构建实体的列表。
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 4d5ce9775e7844fcc82aa993f5b01c7cc7ae4779
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213729"
---
# <a name="entities-per-culture-in-your-luis-model"></a>LUIS 模型中每个区域性的实体

语言理解 (LUIS) 提供了预构建的实体。 当应用程序中包括预构建实体时，LUIS 会在终结点响应中包括对应的实体预测。 所有陈述示例都标记有实体。 **无法**修改预构建实体的行为。 除非另行说明，预构建实体在所有 LUIS 应用程序区域设置（语言区域）中都可用。 下表显示了每个语言区域支持的预构建实体。

|环境|子区域性|
|--|--|
|中文|[zh-CN](#chinese-entity-support)|
|荷兰语|[nl-NL](#dutch-entity-support)|
|英语|[en-US（美国）](#english-american-entity-support)|
|法语|[fr-CA（加拿大）](#french-canadian-entity-support)、[fr-FR（法国）](#french-france-entity-support) |
|德语|[de-DE](#german-entity-support)|
|意大利语|[it-IT](#italian-entity-support)|
|日语|[ja-JP](#japanese-entity-support)|
|韩语|[ko-KR](#korean-entity-support)|
|葡萄牙语|[pt-BR（巴西）](#portuguese-brazil-entity-support)|
|西班牙语|[es-ES（西班牙）](#spanish-spain-entity-support)、[es-MX（墨西哥）](#spanish-mexico-entity-support)|

## <a name="chinese-entity-support"></a>中文实体支持

支持以下实体：

|预生成实体|```zh-CN``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    -   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="dutch-entity-support"></a>荷兰语实体支持

支持以下实体：

|预生成实体|```nl-NL``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    -   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="english-american-entity-support"></a>英语（美国）实体支持

支持以下实体：

|预生成实体|```en-US``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-france-entity-support"></a>法语（法国）实体支持

支持以下实体：

|预生成实体|```fr-FR``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    ✔   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    ✔   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="french-canadian-entity-support"></a>法语（加拿大）实体支持

支持以下实体：

|预生成实体|```fr-CA``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="german-entity-support"></a>德语实体支持

支持以下实体：

|预生成实体|```de-DE``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="italian-entity-support"></a>意大利语实体支持

支持以下实体：

|预生成实体|```it-IT``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="japanese-entity-support"></a>日语实体支持

支持以下实体：

|预生成实体|```ja-JP``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="korean-entity-support"></a>韩语实体支持

支持以下实体：

|预生成实体|```ko-KR``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    -   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    -   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    -   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    -   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    -   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="portuguese-brazil-entity-support"></a>葡萄牙语（巴西）实体支持

支持以下实体：

|预生成实体|```pt-BR``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-spain-entity-support"></a>西班牙语（西班牙）实体支持

支持以下实体：

|预生成实体|```es-ES``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    ✔   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    ✔   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    ✔   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    ✔   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    ✔   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    ✔   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    ✔   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

## <a name="spanish-mexico-entity-support"></a>西班牙语（墨西哥）实体支持

支持以下实体：

|预生成实体|```es-MX``` |
------|:------:|
[存在时长](luis-reference-prebuilt-age.md)：<br>year<br>月份<br>week<br>day   |    -   |
[货币（金钱）](luis-reference-prebuilt-currency.md)：<br>美元<br>分数单位（示例：便士）  |    -   |
[DatetimeV2](luis-reference-prebuilt-datetimev2.md)：<br>日期<br>daterange<br>time<br>时间范围   |    -   | 
[维度](luis-reference-prebuilt-dimension.md)：<br>卷<br>面积<br>重量<br>信息（示例：位/字节）<br>长度（示例：米）<br>速度（示例：英里每小时）  |    -   | 
[电子邮件](luis-reference-prebuilt-email.md)   |    ✔   | 
[GeographyV2](luis-reference-prebuilt-geographyV2.md)   |    -   | 
[keyPhrase](luis-reference-prebuilt-keyphrase.md)   |    ✔   | 
[数字](luis-reference-prebuilt-number.md)   |    ✔   |  
[序号](luis-reference-prebuilt-ordinal.md)   |    -   |  
[百分比](luis-reference-prebuilt-percentage.md)   |    -   | 
[PersonName](luis-reference-prebuilt-person.md)   |    -   | 
[电话号码](luis-reference-prebuilt-phonenumber.md)   |    ✔   | 
[温度](luis-reference-prebuilt-temperature.md)：<br>华氏温度<br>开氏温度<br>兰金温度<br>德利尔温度<br>摄氏温度   |    -   | 
[URL](luis-reference-prebuilt-url.md)   |    ✔   |

请参阅有关[已废弃的预构建实体](luis-reference-prebuilt-deprecated.md)的说明。

KeyPhrase 不适用于葡萄牙语（巴西）```pt-BR``` 的所有子区域性。

## <a name="contribute-to-prebuilt-entity-cultures"></a>为预构建实体语言区域做贡献
预构建实体是在 Recognizers-Text 开发源代码项目中开发的。 [参与](https://github.com/Microsoft/Recognizers-Text)项目。 该项目包括每个语言区域的货币的示例。 

Recognizers-Text 项目中不包括 GeographyV2 和 PersonName。 有关这些预生成实体的问题，请建立[支持请求](../../azure-supportability/how-to-create-azure-support-request.md)。 

## <a name="next-steps"></a>后续步骤

了解[数字](luis-reference-prebuilt-number.md)、[datetimeV2](luis-reference-prebuilt-datetimev2.md) 和[货币](luis-reference-prebuilt-currency.md)实体。 
