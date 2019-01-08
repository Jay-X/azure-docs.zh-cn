---
title: Azure 媒体服务中的 LiveEvent 延迟 | Microsoft Docs
description: 本主题概述了 LiveEvent 延迟，并展示了如何设置较低的延迟。
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: b167d3424d520cf8be515eec9d495164dd9785ab
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682089"
---
# <a name="liveevent-latency-in-media-services"></a>媒体服务中的 LiveEvent 延迟

本文展示了如何在 **LiveEvent** 上设置较低的延迟。 它还讨论了在各种播放器中使用低延迟设置时看到的典型结果。 结果因 CDN 和网络延迟而异。 

若要使用新的 **LowLatency** 功能，请在 **LiveEvent** 上将 **StreamOptionsFlag** 设置为 **LowLatency**。 流启动并运行后，可以使用 [Azure Media Player](http://ampdemo.azureedge.net/) (AMP) 演示页，并设置播放选项以使用“低延迟启发式配置文件”。

以下 .NET 示例展示了如何在 **LiveEvent** 上设置 **LowLatency**：

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second "Group Of Pictures" (GOP) length instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

查看完整示例：[MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126)。

## <a name="liveevents-latency"></a>LiveEvents 延迟

下表显示了媒体服务中的延迟的典型结果（当启用了 LowLatency 标志时），延迟指的是从贡献源到达服务之时到查看者看到播放器上的回放之时之间的时间。 若要以最佳方式使用低延迟，应将编码器设置下调至 1 秒“图片组”(GOP) 长度。 当使用较高的 GOP 长度时，可以在相同帧速率下最小化带宽消耗并降低比特率。 在动作较少的视频中特别有用。

### <a name="pass-through"></a>直通 

||启用 2 秒 GOP 低延迟|启用 1 秒 GOP 低延迟|
|---|---|---|
|AMP 中的 DASH|10 秒|8 秒|
|本机 iOS 播放器上的 HLS|14 秒|10 秒|

### <a name="live-encoding"></a>实时编码

||启用 2 秒 GOP 低延迟|启用 1 秒 GOP 低延迟|
|---|---|---|
|AMP 中的 DASH|14 秒|10 秒|
|本机 iOS 播放器上的 HLS|18 秒|13 秒|

> [!NOTE]
> 根据本地网络状况以及是否引入了 CDN 缓存层，端到端延迟可能有所不同。 应当测试你的确切配置。

## <a name="next-steps"></a>后续步骤

- [实时传送视频流概述](live-streaming-overview.md)
- [实时传送视频流教程](stream-live-tutorial-with-api.md)
