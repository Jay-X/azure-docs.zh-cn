---
title: 在 Azure Active Directory B2C 中选择页面协定 | Microsoft Docs
description: 了解如何在 Azure Active Directory B2C 中选择页面协定。
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 637cdb338496764e64c18a887673808ef4e8415a
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203447"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>使用自定义策略在 Azure Active Directory B2C 中选择页面协定

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

可以通过在[自定义策略](active-directory-b2c-overview-custom.md)中配置页面协定在 Azure Active Directory (Azure AD) B2C 中选择页面协定。 页面协定是 Azure AD B2C 提供的元素与你提供的内容之间的关联。 如果打算使用 [Javascript](javascript-samples.md)，需要为自定义策略中所有的内容定义定义一个页协定版本。

## <a name="replace-datauri-values"></a>替换 DataUri 值

在自定义策略中，可以包含 [ContentDefinitions](contentdefinitions.md)，它定义用户旅程中使用的 HTML 模板。 **ContentDefinition** 包含一个 **DataUri**，它引用 Azure AD B2C 提供的页面元素。 **LoadUri** 是你提供的 HTML 和 CSS 内容的相对路径。

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

要选择页面协定，请在策略中的 [ContentDefinitions](contentdefinitions.md) 中更改 **DataUri** 值。 通过从旧的 **DataUri** 值切换为新值，选择一个不可变包。 使用此包的好处是你知道它不会更改，不会在页面上导致意外的行为。 

若要设置页面协定，请使用下表查找 **DataUri** 值。 

| 旧 DataUri 值 | 新 DataUri 值 |
| ----------------- | ----------------- |
| urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0 | 
| urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0 |
| urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0 | 
| urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0 |
| urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0 |

## <a name="next-steps"></a>后续步骤

有关如何自定义应用程序用户界面的详细信息，请参阅[在 Azure Active Directory B2C 中使用自定义策略自定义应用程序的用户界面](active-directory-b2c-ui-customization-custom.md)。



