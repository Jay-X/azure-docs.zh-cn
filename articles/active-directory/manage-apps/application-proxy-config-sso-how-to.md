---
title: 如何配置应用程序代理应用程序的单一登录 | Microsoft Docs
description: 如何快速配置应用程序代理应用程序的单一登录
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: fa0716aec49d4b3600e524bddb9e8f9ae09c6696
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55203770"
---
# <a name="how-to-configure-single-sign-on-to-an-application-proxy-application"></a>如何配置应用程序代理应用程序的单一登录

单一登录 (SSO) 允许用户访问应用程序，无需进行多次身份验证。 借助 Azure Active Directory，单一登录允许在云中执行一次身份验证，并允许服务或连接器模拟用户，完成来自应用程序的任何其他身份验证质询。

## <a name="how-to-configure-single-sign-on"></a>如何配置单一登录
若要配置 SSO，请先确保应用程序已配置为通过 Azure Active Directory 进行预身份验证。 要执行此配置，请转到“Azure Active Directory” -&gt;“企业应用程序” -&gt;“所有应用程序” -&gt;“你的应用程序”-&gt;“应用程序代理”。 在此页上，会看到“预身份验证”字段，确保将该字段设置为“Azure Active Directory”。 

有关预身份验证方法的详细信息，请参阅[应用发布文档](application-proxy-add-on-premises-application.md)的步骤 4。

   ![Azure 门户中的预身份验证方法](./media/application-proxy-config-sso-how-to/app-proxy.png)

## <a name="configuring-single-sign-on-modes-for-application-proxy-applications"></a>为应用程序代理应用程序配置单一登录模式
配置特定类型的单一登录。 登录方法的分类基于后端应用程序使用的身份验证类型。 应用代理应用程序支持三种类型的登录：

-   **基于密码的登录**：基于密码的登录可用于任何使用用户名和密码字段进行登录的应用程序。 若要了解配置步骤，请参阅[配置 Azure AD 库应用程序的密码单一登录](configure-password-single-sign-on-gallery-applications.md)。

-   **集成 Windows 身份验证**：对于使用集成 Windows 身份验证 (IWA) 的应用程序，通过 Kerberos 约束委派 (KCD) 启用单一登录。 此方法在 Active Directory 中赋予应用程序代理连接器权限来模拟用户，并代表用户发送和接收令牌。 关于配置 KCD 的详细信息，可以在[通过 KCD 启用单一登录文档](application-proxy-configure-single-sign-on-with-kcd.md)中找到。

-   **基于标头的登录**：通过合作关系启用基于标头的登录，需要提供一些其他配置。 有关为使用标头进行身份验证的应用程序配置单一登录的合作关系和分步说明的详细信息，请参阅 [Azure AD 的 PingAccess 文档](application-proxy-configure-single-sign-on-with-ping-access.md)。

可通过以下方式找到这些选项：转到“企业应用程序”中的应用程序，打开左侧菜单上的“单一登录”页。 请注意，如果应用程序是在旧门户中创建，可能看不到所有这些选项。

在此页上，还可以看到另一个登录选项：链接登录。 此选项也受应用程序代理支持。 但是，此选项不会将单一登录添加到应用程序。 也就是说，应用程序可能已经使用另一个服务（如 Active Directory 联合身份验证服务）实现了单一登录。 

此选项允许管理员在访问应用程序时创建一个指向用户首次登录的应用程序的链接。 例如，如果某一个应用程序配置为使用 Active Directory 联合身份验证服务 2.0 验证用户身份，则管理员可以使用“链接登录”选项在访问面板上创建指向该应用程序的链接。

## <a name="next-steps"></a>后续步骤
[使用应用程序代理为应用提供单一登录](application-proxy-configure-single-sign-on-with-kcd.md)
