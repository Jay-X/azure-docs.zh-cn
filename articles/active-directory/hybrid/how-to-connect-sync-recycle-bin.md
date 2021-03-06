---
title: Azure AD Connect 同步：启用 AD 回收站 | Microsoft Docs
description: 本主题推荐将 AD 回收站功能与 Azure AD Connect 一起使用。
services: active-directory
keywords: AD 回收站, 意外删除, 源定位点
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 8580d1c938908d5dd5c962d42d4edb2673ae4f5f
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494368"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect 同步：启用 AD 回收站
建议为同步到 Azure AD 的本地 Active Directory 启用 AD 回收站功能。 

如果意外删除了本地 AD 用户对象并使用该功能进行还原，Azure AD 将还原相应的 Azure AD 用户对象。  有关 AD 回收站功能的信息，请参阅文章 [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx)（还原已删除 Active Directory 对象的方案概述）。

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>启用 AD 回收站的好处
此功能可通过执行以下操作，帮助还原 Azure AD 用户对象：

* 如果意外删除了本地 AD 用户对象，会在下一同步周期中删除相应的 Azure AD 用户对象。 默认情况下，Azure AD 会以软删除状态保存已删除的 Azure AD 用户对象 30 天。

* 如果已启用本地 AD 回收站功能，则可还原已删除的本地 AD 用户对象，而不更改其源定位点值。 将恢复的本地 AD 用户对象同步到 Azure AD 后，Azure AD 将还原已软删除的相应 Azure AD 用户对象。 有关源定位点特性的信息，请参阅文章 [Azure AD Connect：设计概念](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor)。

* 如果没有启用本地 AD 回收站功能，可能需要创建一个 AD 用户对象来替换已删除的对象。 如果将 Azure AD Connect Synchronization Service 配置为对源定位点特性使用系统生成的 AD 特性（如 ObjectGuid），则新创建的 AD 用户对象与已删除的 AD 用户对象所具有的源定位点值会不同。 将新创建的 AD 用户对象同步到 Azure AD 中时，Azure AD 会创建一个新的 Azure AD 用户对象，而不是还原已软删除的 Azure AD 用户对象。

> [!NOTE]
> 默认情况下，Azure AD 会以软删除状态保留已删除的 Azure AD 用户对象 30 天，然后才会将其永久删除。 但管理员可加快此类对象的删除。 对象永久删除后，即使已启用本地 AD 回收站功能，也无法再进行恢复。

## <a name="next-steps"></a>后续步骤
**概述主题**

* [Azure AD Connect 同步：了解和自定义同步](how-to-connect-sync-whatis.md)

* [将本地标识与 Azure Active Directory 集成](whatis-hybrid-identity.md)
