---
title: Azure Active Directory Identity Protection 常见问题解答 | Microsoft Docs
description: 有关 Azure AD Identity Protection 的常见问题解答
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: markvi
ms.reviewer: raluthra
ms.openlocfilehash: 1e8c773b1a6be3e06cde8cd8ff387333548a8918
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2019
ms.locfileid: "55700785"
---
# <a name="azure-active-directory-identity-protection-faq"></a>Azure Active Directory Identity Protection 常见问题解答

本文包括了对 Azure Active Directory (Azure AD) 标识保护常见问题的解答。 有关详细信息，请参阅 [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)。 


## <a name="why-do-some-risk-events-have-closed-system-status"></a>为何某些风险事件具有“已关闭(系统)”状态？

**答：** 这些是 Identity Protection 检测到的风险事件，之所以稍后关闭是因为已不再认为这些事件有风险。 这些事件不会计入用户的风险级别。 

---

## <a name="do-i-need-to-be-a-global-admin-to-use-identity-protection-in-the-azure-portal"></a>是否需要是全局管理员，才能在 Azure 门户中使用 Identity Protection？
**答：** 不是。 若要使用 Identity Protection，可以是安全读者、安全管理员或全局管理员。

---

## <a name="how-do-i-get-identity-protection"></a>如何获取 Identity Protection？

**答：** 请参阅 [Azure Active Directory Premium 入门](../fundamentals/active-directory-get-started-premium.md)获取此问题的解答。

---

## <a name="how-can-i-sort-users-in-users-flagged-for-risk"></a>如何对“已标记为存在风险的用户”中的用户进行排序？

**答：** 在“已标记为存在风险的用户”页面顶部单击“下载”，下载“已标记为存在风险的用户”报表 。 然后即可基于可用字段（包括“上次更新时间 [UTC]”），对下载的数据进行排序。

---
