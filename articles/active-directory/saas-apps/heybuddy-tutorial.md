---
title: 教程：Azure Active Directory 与 HeyBuddy 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 与 HeyBuddy 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d51b5af6-018e-4678-9a3f-b70438394f67
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2018
ms.author: jeedes
ms.openlocfilehash: 123b74298ebb01e8ebcfd70d023b4e71a879e7f9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197885"
---
# <a name="tutorial-azure-active-directory-integration-with-heybuddy"></a>教程：Azure Active Directory 与 HeyBuddy 集成

本教程介绍如何将 HeyBuddy 与 Azure Active Directory (Azure AD) 集成。

将 HeyBuddy 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 HeyBuddy。
- 可以让用户使用其 Azure AD 帐户自动登录到 HeyBuddy（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 HeyBuddy 的集成，需要以下项：

- Azure AD 订阅
- 启用了 HeyBuddy 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述

在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 HeyBuddy
2. 配置和测试 Azure AD 单一登录

## <a name="adding-heybuddy-from-the-gallery"></a>从库中添加 HeyBuddy

若要配置 HeyBuddy 与 Azure AD 的集成，需要从库中将 HeyBuddy 添加到托管 SaaS 应用列表。

**若要从库中添加 HeyBuddy，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

2. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

4. 在搜索框中，键入“HeyBuddy”，在结果面板中选择“HeyBuddy”，然后单击“添加”按钮添加该应用程序。

    ![结果列表中的 HeyBuddy](./media/heybuddy-tutorial/tutorial_heybuddy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分，我们基于一个名为“Britta Simon”的测试用户使用 HeyBuddy 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 HeyBuddy 用户。 换句话说，需要建立 Azure AD 用户与 HeyBuddy 中相关用户之间的链接关系。

若要配置和测试 HeyBuddy 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
2. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
3. **[创建 HeyBuddy 测试用户](#creating-a-heybuddy-test-user)** - 在 HeyBuddy 中创建 Britta Simon 的对应用户，将其链接到该用户的 Azure AD 表示形式。
4. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
5. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分，我们将在 Azure 门户中启用 Azure AD 单一登录并在 HeyBuddy 应用程序中配置单一登录。

**若要配置 HeyBuddy 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中，在 **HeyBuddy** 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

2. 在“选择单一登录方法”对话框中，单击“SAML”模式对应的“选择”，以启用单一登录。

    ![配置单一登录](common/tutorial_general_301.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框。

    ![配置单一登录](common/editconfigure.png)

4. 在“基本 SAML 配置”部分中，按照以下步骤操作：

    ![HeyBuddy 域和 URL 单一登录信息](./media/heybuddy-tutorial/tutorial_heybuddy_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL：`https://api.heybuddy.com/auth/<ENTITY ID>`

    b. 在“标识符(实体 ID)”文本框中，使用以下模式键入 URL： `YourCompanyInstanceofHeyBuddy`

    > [!NOTE]
    > 这些不是实际值。 使用实际登录 URL 和标识符（实体 ID）更新这些值。 “登录 URL”中的 `Entity ID` 是为每个组织自动生成的。 请联系 [HeyBuddy 客户端支持团队](mailto:support@heybuddy.com)获取这些值。

5. HeyBuddy 应用程序需要特定格式的 SAML 断言。 请为此应用程序配置以下声明。 可以在应用程序集成页的“用户属性和声明”部分管理这些属性的值。 在“使用 SAML 设置单一登录”页上，单击“编辑”按钮以打开“用户属性和声明”对话框。

    ![图像](./media/heybuddy-tutorial/tutorial_heybuddy_attribute.png)

    >[!NOTE]
    >有关如何配置和设置应用程序的角色，请参阅此[链接](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management)。

6. 在“用户属性和声明”对话框的“用户声明”部分中，按上图所示配置 SAML 令牌属性，并执行以下步骤：
    
    | Name  | 源属性  |
    | --------------- | --------------- |
    | 角色               | user.assignedroles |

    a. 单击“添加新声明”以打开“管理用户声明”对话框。

    ![图像](./common/new_save_attribute.png)

    ![图像](./common/new_attribute_details.png)

    b. 在“名称”文本框中，键入为该行显示的属性名称。

    c. 将“命名空间”值保留空白。

    d. 选择“源”作为“属性”。

    e. 在“源属性”列表中，键入为该行显示的属性值。

    f. 单击“ **保存**”。

7. 在“SAML 签名证书”页的“SAML 签名证书”部分中，单击复制**图标**来复制应用联合元数据 URL，并将其粘贴到记事本 **。** 

    ![证书下载链接](./media/heybuddy-tutorial/tutorial_heybuddy_certificate.png) 

8. 若要在 **HeyBuddy** 端配置单一登录，需要将复制的“应用联合元数据 URL”发送给 [HeyBuddy 支持团队](mailto:support@heybuddy.com)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![创建 Azure AD 用户][100]

2. 选择屏幕顶部的“新建用户”。

    ![创建 Azure AD 测试用户](common/create_aaduser_01.png) 

3. 在“用户属性”中，按照以下步骤操作。

    ![创建 Azure AD 测试用户](common/create_aaduser_02.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在“用户名”字段中键入 brittasimon@yourcompanydomain.extension  
    例如： BrittaSimon@contoso.com

    c. 选择“属性”，再选择“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 选择“创建”。

### <a name="creating-a-heybuddy-test-user"></a>创建 HeyBuddy 测试用户

本部分的目的是在 HeyBuddy 中创建名为“Britta Simon”的用户。 HeyBuddy 支持在默认情况下启用的实时预配。 此部分不存在任何操作项。 尝试访问 HeyBuddy 期间，如果尚不存在用户，则会创建一个新用户。
>[!Note]
>如果需要手动创建用户，请联系  [HeyBuddy 支持团队](mailto:support@heybuddy.com)。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分，我们通过向 Britta Simon 授予 HeyBuddy 的访问权限，使其能够使用 Azure 单一登录。

1. 在 Azure 门户中，选择“企业应用程序”，然后选择“所有应用程序”。

    ![分配用户][201]

2. 在应用程序列表中，选择“HeyBuddy”。

    ![配置单一登录](./media/heybuddy-tutorial/tutorial_heybuddy_app.png)

3. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202]

4. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 在“添加分配”对话框中，选择“分配”按钮。

### <a name="testing-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击 HeyBuddy 磁贴时，应会自动登录到 HeyBuddy 应用程序。
有关访问面板的详细信息，请参阅[访问面板简介](../user-help/active-directory-saas-access-panel-introduction.md)。

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
