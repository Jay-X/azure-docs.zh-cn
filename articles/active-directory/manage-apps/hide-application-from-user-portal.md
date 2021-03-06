---
title: 使应用程序不出现在用户在 Azure Active Directory 中的体验中 | Microsoft Docs
description: 如何使应用程序不出现在用户在 Azure Active Directory 访问面板或 Office 365 启动器的体验中。
services: active-directory
author: barbkess
manager: daveba
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: barbkess
ms.reviewer: kasimpso
ms.openlocfilehash: d9745dac425652e571c547a9f0c0ee3ba0d1073c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55195369"
---
# <a name="hide-applications-from-end-users-in-azure-active-directory"></a>在 Azure Active Directory 中向最终用户隐藏应用程序

有关如何在最终用户的 MyApps 面板或 Office 365 启动器中隐藏应用程序的说明。 当应用程序隐藏后，用户仍然拥有对应用程序的权限。 

## <a name="prerequisites"></a>先决条件

在 MyApps 面板和 Office 365 启动器中隐藏应用程序需要应用程序管理员权限。

隐藏所有 Office 365 应用程序需要全局管理员权限。


## <a name="hide-an-application-from-the-end-user"></a>向最终用户隐藏应用程序
按照以下步骤在 MyApps 面板和 Office 365 应用程序启动器中隐藏应用程序。

1.  以目录的全局管理员身份登录到 [Azure 门户](https://portal.azure.com)。
2.  选择“Azure Active Directory”。
3.  选择“企业应用程序”。 “企业应用程序 - 所有应用程序”边栏选项卡随即打开。
4.  在“应用程序类型”下，选择“企业应用程序”（如果尚未选择）。
5.  搜索要隐藏的应用程序，然后单击该应用程序。  此时会打开应用程序的概述。
6.  单击“属性”。 
7.  对于“对用户可见?”问题，单击“否”。
8.  单击“ **保存**”。


## <a name="hide-office-365-applications-from-the-myapps-panel"></a>在 MyApps 面板中隐藏 Office 365 应用程序

按照以下步骤在 MyApps 面板中隐藏所有 Office 365 应用程序。 应用程序在 Office 365 门户中仍然可见。

1.  以目录的全局管理员身份登录到 [Azure 门户](https://portal.azure.com)。
2.  选择“Azure Active Directory”。
3.  选择“用户设置”。
4.  在“企业应用程序”下，单击“管理最终用户启动和查看其应用程序的方式”。
5.  对于“用户只能在 Office 365 门户中查看 Office 365 应用”，单击“是”。
6.  单击“ **保存**”。


## <a name="next-steps"></a>后续步骤
* [查看所有组](../fundamentals/active-directory-groups-view-azure-portal.md)
* [向企业应用分配用户或组](assign-user-or-group-access-portal.md)
* [删除企业应用的用户或组分配](remove-user-or-group-access-portal.md)
* [更改企业应用的名称或徽标](change-name-or-logo-portal.md)

