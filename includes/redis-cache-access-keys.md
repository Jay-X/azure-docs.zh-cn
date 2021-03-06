---
title: include 文件
description: include 文件
services: redis-cache
author: wesmc7777
ms.service: cache
ms.topic: include
ms.date: 03/28/2018
ms.author: wesmc
ms.custom: include file
ms.openlocfilehash: 50264dfba401f623c4ad14a6310303f2787fc85f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111758"
---
### <a name="retrieve-host-name-ports-and-access-keys-by-using-the-azure-portal"></a>使用 Azure 门户检索主机名、端口和访问密钥

连接到某个 Azure Redis 缓存实例时，缓存客户端需要该缓存的主机名、端口和密钥。 在某些客户端中，这些项的名称可能略有不同。 可以在 Azure 门户中检索此信息。

#### <a name="to-retrieve-the-access-keys-and-host-name"></a>检索访问密钥和主机名的步骤

1. 若要使用 [Azure 门户](https://portal.azure.com)检索访问密钥，请浏览到缓存，然后选择“访问密钥”。 

    ![Azure Redis 缓存密钥](media/redis-cache-access-keys/redis-cache-keys.png)

2. 若要检索主机名和端口，请选择“属性”。

    ![Azure Redis 缓存属性](media/redis-cache-access-keys/redis-cache-hostname-ports.png)

