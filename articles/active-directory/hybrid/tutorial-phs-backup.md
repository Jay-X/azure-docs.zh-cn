---
title: 教程：在 Azure AD Connect 中将 PHS 设置为 AD FS 的后备 | Microsoft Docs
description: 演示如何启用密码哈希同步作为 AD FS 的后备。
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 01/30/2019
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 7e7c9ca30659fd1e99989bb77406b653a8ed9e7f
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302908"
---
# <a name="tutorial--setting-up-phs-as-backup-for-ad-fs-in-azure-ad-connect"></a>教程：在 Azure AD Connect 中将 PHS 设置为 AD FS 的后备

下面的教程将指导你设置密码哈希同步作为 AD FS 的后备和故障转移。  本文档还将演示在 AD FS 失败或不可用的情况下，如何启用密码哈希同步作为主要身份验证方法。

## <a name="prerequisites"></a>先决条件
本教程以[教程：将单个 AD 林环境与云联合起来](tutorial-federation.md)为基础，在尝试本教程之前，必须完成此教程。  如果尚未完成，请在尝试本文档中的步骤之前完成该教程。

## <a name="enable-phs-in-azure-ad-connect"></a>在 Azure AD Connect 中启用 PHS
我们已经有一个使用联合身份验证的 Azure AD Connect 环境，那么第一步就是打开密码哈希同步，并允许 Azure AD Connect 同步哈希。

请执行以下操作：

1.  双击在桌面上创建的 Azure AD Connect 图标
2.  单击“配置”。
3.  从“其他任务”页中选择“自定义同步选项”，并单击“下一步”。
4.  输入全局管理员的用户名和密码。  上一教程中已在[此处](tutorial-federation.md#create-a-global-administrator-in-azure-ad)创建该帐户。
5.  在“连接目录”屏幕上，单击“下一步”。
6.  在“域和 OU 筛选”屏幕上，单击“下一步”。
7.  在“可选功能”屏幕上，选中“密码哈希同步”并单击“下一步”。
![选择](media/tutorial-phs-backup/backup1.png)</br>
8.  在“准备好配置”屏幕上，单击“配置”。
9.  一旦配置完成，单击“退出”。
10. 就这么简单！  完成。  密码哈希同步即将发生，如果 AD FS 变得不可用，即可用作后备。

## <a name="switch-to-password-hash-synchronization"></a>切换到密码哈希同步
现在，我们将演示如何在需要时切换到密码哈希同步。  请执行以下操作：

1. 双击在桌面上创建的 Azure AD Connect 图标
2.  单击“配置”。
3.  选择“更改用户登录”并单击“下一步”。
![更改](media/tutorial-phs-backup/backup2.png)</br>
4.  输入全局管理员的用户名和密码。  上一教程中已在[此处](tutorial-federation.md#create-a-global-administrator-in-azure-ad)创建该帐户。
5.  在“用户登录”屏幕上，选择“密码哈希同步”，并选中“请勿切换用户帐户”框。  
6.  保留默认选中的“启用单一登录”并单击“下一步”。
7.  在“启用单一登录”屏幕上单击“下一步”。
8.  在“准备好配置”屏幕上，单击“配置”。
9.  配置完成后，单击“退出”。
10. 用户现在可以使用他们的密码登录到 Azure 和 Azure 服务。

## <a name="test-signing-in-with-one-of-our-users"></a>使用我们的某位用户测试登录

1.  浏览到 [http://myapps.microsoft.com](http://myapps.microsoft.com)
2. 使用在我们的新租户中创建的用户帐户登录。  需要使用以下格式登录：(user@domain.onmicrosoft.com)。 使用用户用于在本地登录的相同密码。</br>
![验证](media/tutorial-password-hash-sync/verify1.png)</br>

## <a name="next-steps"></a>后续步骤


- [硬件和先决条件](how-to-connect-install-prerequisites.md) 
- [快速设置](how-to-connect-install-express.md)
- [密码哈希同步](how-to-connect-password-hash-synchronization.md)|
