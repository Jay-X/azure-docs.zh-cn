---
title: 常见参数和标头
description: 这些参数和标头常见于与 Key Vault 资源相关的所有操作。
services: key-vault
documentationcenter: ''
author: bryanla
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: a715d13ca9-d6e8-4e54-ac5e-0ed9400fb15b15d13ca9-d6e8-4e54-ac5e-0ed9400fb15b
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: bryanla
ms.openlocfilehash: de9243a0a95c14a048be124976b07853a48a9a08
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54075220"
---
# <a name="common-parameters-and-headers"></a>常见参数和标头

以下信息常见于与 Key Vault 资源相关的所有操作：

- 将 `{api-version}` 替换为 URI 中的 API 版本。
- 将 `{subscription-id}` 替换为 URI 中的订阅标识符
- 将 `{resource-group-name}` 替换为资源组。 有关详细信息，请参阅“使用资源组管理 Azure 资源”。
- 将 `{vault-name}` 替换为 URI 中 Key Vault 的名称。
- 将 Content-Type 表头设置为 application/json。
- 将授权标头设置为从 Azure Active Directory (AAD) 获取的 JSON Web 令牌。 有关详细信息，请参阅[对 Azure 资源管理器请求进行身份验证](authentication-requests-and-responses.md)。

## <a name="common-error-response"></a>常见错误响应
服务使用 HTTP 状态代码指示成功或失败。 此外，失败还会包含采用以下格式的响应：

   {  
     "error": {  
     "code":"BadRequest",  
     "message":"The key vault sku is invalid."  
     }  
   }  

|元素名称 | 类型 | Description |
|---|---|---|
| 代码 | 字符串 | 发生的错误类型。|
| message | 字符串 | 错误原因说明。 |



## <a name="see-also"></a>另请参阅
 [Azure Key Vault REST API 参考](/rest/api/keyvault/)
