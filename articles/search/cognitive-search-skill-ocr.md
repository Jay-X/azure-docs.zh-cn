---
title: OCR 认知搜索技能 - Azure 搜索
description: 在 Azure 搜索扩充管道中使用光学字符识别 (OCR) 从图像文件中提取文本。
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 01/17/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 485837d6235f4868665df2907c01fa1bd26db357
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766361"
---
# <a name="ocr-cognitive-skill"></a>OCR 认知技能

光学字符识别 (OCR) 技能可识别图像文件中的印刷体文本和手写文本。 此技能使用认知服务中的[计算机视觉](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home)提供的机器学习模型。 **OCR** 技能将映射到以下功能：

+ 当 textExtractionAlgorithm 设置为“handwritten”时，将使用[“RecognizeText”](../cognitive-services/computer-vision/quickstarts-sdk/csharp-hand-text-sdk.md)功能。
+ 当 textExtractionAlgorithm 设置为“printed”时，[“OCR”](../cognitive-services/computer-vision/concept-extracting-text-ocr.md)功能用于英语以外的语言。 对于英语，使用新的[“识别文本”](../cognitive-services/computer-vision/concept-recognizing-text.md)功能输出文本。

OCR 技能可以从图像文件中提取文本。 支持的文件格式包括：

+ .JPEG
+ .JPG
+ .PNG
+ .BMP
+ .GIF

> [!NOTE]
> 从 2018 年 12 月 21 日开始，可以[将认知服务资源附加到 Azure 搜索技能集](cognitive-search-attach-cognitive-services.md)。 这会使我们能够开始对技能集执行收费。 在此日期，我们还会开始将图像提取视为文档破解阶段的一部分进行计费。 我们将继续提供文档文本提取服务而不收取额外费用。
>
> 执行[内置认知技能](cognitive-search-predefined-skills.md)按[认知服务即用即付价格](https://azure.microsoft.com/pricing/details/cognitive-services)收费，其费率与你以往直接执行任务的费率相同。 图像提取是 Azure 搜索收费项，目前以预览定价提供。 有关详细信息，请参见 [Azure 搜索定价页](https://go.microsoft.com/fwlink/?linkid=2042400)或[如何计费](search-sku-tier.md#how-billing-works)。

## <a name="skill-parameters"></a>技能参数

参数区分大小写。

| 参数名称     | 说明 |
|--------------------|-------------|
| detectOrientation | 启用图像方向自动检测。 <br/> 有效值：true / false。|
|defaultLanguageCode | <p>  输入文本的语言代码。 支持的语言包括： <br/> zh-Hans（中文简体） <br/> zh-Hant（中文繁体） <br/>cs（捷克语） <br/>da（丹麦语） <br/>nl（荷兰语） <br/>en（英语） <br/>fi（芬兰语）  <br/>fr（法语） <br/>  de（德语） <br/>el（希腊语） <br/> hu（匈牙利） <br/> it（意大利语） <br/>  ja（日语） <br/> ko（韩语） <br/> nb（挪威语） <br/>   pl（波兰语） <br/> pt（葡萄牙语） <br/>  ru（俄语） <br/>  es（西班牙语） <br/>  sv（瑞典语） <br/>  tr（土耳其语） <br/> ar（阿拉伯语） <br/> ro（罗马尼亚语） <br/> sr-Cyrl（塞尔维亚语西里尔文） <br/> sr-Latn（塞尔维亚语拉丁语） <br/>  sk（斯洛伐克语） <br/>  unk（未知） <br/><br/> 如果语言代码未指定或为 null，则语言将设置为英语。 如果语言显式设置为“unk”，则将自动检测语言。 </p> |
| textExtractionAlgorithm | “打印”或“手写”。 “手写”文本识别 OCR 算法当前处于预览状态，仅支持英语。 |

## <a name="skill-inputs"></a>技能输入

| 输入名称      | 说明                                          |
|---------------|------------------------------------------------------|
| 图像         | 复杂类型。 当前仅适用于“/document/normalized_images”字段，当 ```imageAction``` 设置为 ```generateNormalizedImages``` 时由 Azure Blob 索引器生成。 请参阅[此示例](#sample-output)获取详细信息。|


## <a name="skill-outputs"></a>技能输出
| 输出名称     | 说明                   |
|---------------|-------------------------------|
| text          | 从映像中提取的纯文本。   |
| layoutText    | 描述提取的文本以及找到文本的位置的复杂类型。|


## <a name="sample-definition"></a>示例定义

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>示例文本和 layoutText 输出

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>示例：将从嵌入图像中提取的文本与文档内容合并。

文本合并的一个常见用例是将图像的文本表示形式（OCR 技能中的文本或图像的描述文字）合并到文档的内容字段中。 

以下示例技能集创建 *merged_text* 字段。 此字段包含文档的文本内容以及该文档中嵌入的每个图像的 OCRed 文本。 

#### <a name="request-body-syntax"></a>请求正文语法
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```
以上技能集示例假设存在标准化的图像字段。 要生成此字段，请将索引器定义中的 imageAction 配置设置为 generateNormalizedImages，如下所示：

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>另请参阅
+ [预定义的技能](cognitive-search-predefined-skills.md)
+ [TextMerger 技能](cognitive-search-skill-textmerger.md)
+ [如何定义技能集](cognitive-search-defining-skillset.md)
+ [创建索引器 (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
