---
title: 教程：Azure Active Directory 与 SAML SSO for Jira by resolution GmbH 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 SAML SSO for Jira by resolution GmbH 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 20e18819-e330-4e40-bd8d-2ff3b98e035f
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/03/2018
ms.author: jeedes
ms.openlocfilehash: adef572d9954443f24cfd1ab13c8c8a2e0ffa830
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811802"
---
# <a name="tutorial-azure-active-directory-integration-with-saml-sso-for-jira-by-resolution-gmbh"></a>教程：Azure Active Directory 与 SAML SSO for Jira by resolution GmbH 集成

本教程介绍如何将 SAML SSO for Jira by resolution GmbH 与 Azure Active Directory (Azure AD) 集成。
将 SAML SSO for Jira by resolution GmbH 与 Azure AD 集成具有以下优势：

* 可以在 Azure AD 中控制谁有权访问 SAML SSO for Jira by resolution GmbH。
* 可以让用户通过其 Azure AD 帐户自动登录到 SAML SSO for Jira by resolution GmbH（单一登录）。
* 可在中心位置（即 Azure 门户）管理帐户。

如果要了解有关 SaaS 应用与 Azure AD 集成的更多详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。
如果还没有 Azure 订阅，可以在开始前[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 SAML SSO for Jira by resolution GmbH 的集成，需备齐以下项目：

* 一个 Azure AD 订阅。 如果你没有 Azure AD 环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。
* 启用了 SAML SSO for Jira by resolution GmbH 单一登录的订阅

## <a name="scenario-description"></a>方案描述

本教程会在测试环境中配置和测试 Azure AD 单一登录。

* SAML SSO for Jira by resolution GmbH 支持 **SP** 和 **IDP** 发起的 SSO

## <a name="adding-saml-sso-for-jira-by-resolution-gmbh-from-the-gallery"></a>从库添加 SAML SSO for Jira by resolution GmbH

要配置 SAML SSO for Jira by resolution GmbH 的集成（集成到 Azure AD 中），需从库中将 SAML SSO for Jira by resolution GmbH 添加到托管 SaaS 应用的列表中。

**若要从库中添加 SAML SSO for Jira by resolution GmbH，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。

    ![“Azure Active Directory”按钮](common/select-azuread.png)

2. 转到“企业应用”，并选择“所有应用”选项。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

3. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮](common/add-new-app.png)

4. 在搜索框中，键入 SAML SSO for Jira by resolution GmbH，在结果面板中选择 SAML SSO for Jira by resolution GmbH，然后单击“添加”按钮以添加该应用程序。

     ![结果列表中的 SAML SSO for Jira by resolution GmbH](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 SAML SSO for Jira by resolution GmbH 的 Azure AD 单一登录。
若要运行单一登录，需要在 Azure AD 用户与 SAML SSO for Jira by resolution GmbH 相关用户之间建立链接关系。

若要配置并测试 SAML SSO for Jira by resolution GmbH 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
2. **[配置 SAML SSO for Jira by resolution GmbH 单一登录](#configure-saml-sso-for-jira-by-resolution-gmbh-single-sign-on)** - 在应用程序端配置单一登录设置。
3. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
4. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
5. **[创建 SAML SSO for Jira by resolution GmbH 测试用户](#create-saml-sso-for-jira-by-resolution-gmbh-test-user)** - 在 SAML SSO for Jira by resolution GmbH 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
6. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录。

若要配置 SAML SSO for Jira by resolution GmbH 的 Azure AD 单一登录，请执行以下步骤：

1. 在 [Azure 门户](https://portal.azure.com/)中的“SAML SSO for Jira by resolution GmbH”应用程序集成页上，选择“单一登录”。

    ![配置单一登录链接](common/select-sso.png)

2. 在**选择单一登录方法**对话框中，选择 **SAML/WS-Fed**模式以启用单一登录。

    ![单一登录选择模式](common/select-saml-option.png)

3. 在“使用 SAML 设置单一登录”页上，单击“编辑”图标以打开“基本 SAML 配置”对话框。

    ![编辑基本 SAML 配置](common/edit-urls.png)

4. 如果要在 IDP 发起的模式下配置应用程序，请在“基本 SAML 配置部分”中执行以下步骤：

    ![SAML SSO for Jira by resolution GmbH 域和 URL 单一登录信息](common/idp-intiated.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://<server-base-url>/plugins/servlet/samlsso`

    b. 在“回复 URL”文本框中，使用以下模式键入 URL：`https://<server-base-url>/plugins/servlet/samlsso`

    c. 如果要在 SP 发起的模式下配置应用程序，请单击“设置其他 URL”，并执行以下步骤：

    ![SAML SSO for Jira by resolution GmbH 域和 URL 单一登录信息](common/metadata-upload-additional-signon.png)

    在“登录 URL”文本框中，使用以下模式键入 URL：`https://<server-base-url>/plugins/servlet/samlsso`

    > [!NOTE]
    > 这些不是实际值。 请使用实际的“标识符”、“回复 URL”和“登录 URL”更新这些值。 若要获取这些值，请联系 [SAML SSO for Jira by resolution GmbH 客户支持团队](https://www.resolution.de/go/support)。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

4. 在“使用 SAML 设置单一登录”页的“SAML 签名证书”部分，单击“下载”以根据要求下载从给定选项提供的“联合元数据 XML”并将其保存在计算机上。

    ![证书下载链接](common/metadataxml.png)

6. 在“设置 SAML SSO for Jira by resolution GmbH”部分，根据要求复制相应的 URL。

    ![复制配置 URL](common/copy-configuration-urls.png)

    a. 登录 URL

    b. Azure AD 标识符

    c. 注销 URL

### <a name="configure-saml-sso-for-jira-by-resolution-gmbh-single-sign-on"></a>配置 SAML SSO for Jira by resolution GmbH 单一登录

1. 在另一个 Web 浏览器窗口中，以管理员身份登录到 **SAML SSO for Jira by resolution GmbH 管理门户**。

2. 将鼠标悬停在小齿轮上，并单击“外接程序”。
    
    ![配置单一登录](./media/samlssojira-tutorial/addon1.png)

3. 系统会你将重定向到“管理员访问权限”页。 输入密码，并单击“确认”按钮。

    ![配置单一登录](./media/samlssojira-tutorial/addon2.png)

4. 在“外接程序”选项卡部分，单击“查找新外接程序”。 搜索“SAML Single Sign On (SSO) for JIRA”，并单击“安装”按钮安装新的 SAML 插件。

    ![配置单一登录](./media/samlssojira-tutorial/addon7.png)

5. 插件安装随即开始。 单击“**关闭**”。

    ![配置单一登录](./media/samlssojira-tutorial/addon8.png)

    ![配置单一登录](./media/samlssojira-tutorial/addon9.png)

6.  单击“管理”。

    ![配置单一登录](./media/samlssojira-tutorial/addon10.png)
    
8. 单击“配置”配置新的插件。

    ![配置单一登录](./media/samlssojira-tutorial/addon11.png)

9. 在“SAML 单一登录插件配置”页上单击“添加新 IdP”按钮，配置标识提供者的设置。

    ![配置单一登录](./media/samlssojira-tutorial/addon4.png)

10. 在“选择 SAML 标识提供者”页上，执行以下步骤：

    ![配置单一登录](./media/samlssojira-tutorial/addon5a.png)
 
    a. 设置“Azure AD”作为 IdP 类型。
    
    b. 添加标识提供者（例如 Azure AD）的“名称”。
    
    c. 添加标识提供者（例如 Azure AD）的“说明”。
    
    d. 单击“下一步”。
    
11. 在“标识提供者配置”页上，单击“下一步”按钮。

    ![配置单一登录](./media/samlssojira-tutorial/addon5b.png)

12. 在“导入 SAML IdP 元数据”页上，执行以下步骤：

    ![配置单一登录](./media/samlssojira-tutorial/addon5c.png)

    a. 单击“加载文件”按钮，然后选择在步骤 5 中下载的元数据 XML 文件。

    b. 单击“导入”按钮。
    
    c. 在导入成功之前，请等待片刻。
    
    d. 单击“下一步”按钮。
    
13. 在“用户 ID 属性和转换”页上，单击“下一步”按钮。

    ![配置单一登录](./media/samlssojira-tutorial/addon5d.png)
    
14. 在“用户创建和更新”页上，单击“保存并下一步”保存设置。   
    
    ![配置单一登录](./media/samlssojira-tutorial/addon6a.png)
    
15. 在“测试设置”页上，单击“跳过测试并手动配置”暂时跳过用户测试。 此测试将在下一部分中执行，并需要在 Azure 门户中进行某些设置。 
    
    ![配置单一登录](./media/samlssojira-tutorial/addon6b.png)
    
16. 在出现的显示“跳过测试意味着...”对话框中，单击“确定”。
    
    ![配置单一登录](./media/samlssojira-tutorial/addon6c.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户 

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”。

    ![“用户和组”以及“所有用户”链接](common/users.png)

2. 选择屏幕顶部的“新建用户”。

    ![“新建用户”按钮](common/new-user.png)

3. 在“用户属性”中，按照以下步骤操作。

    ![“用户”对话框](common/user-properties.png)

    a. 在“名称”字段中，输入 BrittaSimon。
  
    b. 在“用户名”字段中键入 brittasimon@yourcompanydomain.extension  
    例如： BrittaSimon@contoso.com

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 SAML SSO for Jira by resolution GmbH 的权限，允许她使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”、“SAML SSO for Jira by resolution GmbH”。

    ![“企业应用程序”边栏选项卡](common/enterprise-applications.png)

2. 在应用程序列表中，键入并选择“SAML SSO for Jira by resolution GmbH”。

    ![应用程序列表中的 SAML SSO for Jira by resolution GmbH 链接](common/all-applications.png)

3. 在左侧菜单中，选择“用户和组”。

    ![“用户和组”链接](common/users-groups-blade.png)

4. 单击“添加用户”按钮，然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格](common/add-assign-user.png)

5. 在“用户和组”对话框中，选择“用户”列表中的 Britta Simon，然后单击屏幕底部的“选择”按钮。

6. 如果你在 SAML 断言中需要任何角色值，请在“选择角色”对话框中从列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。

7. 在“添加分配”对话框中，单击“分配”按钮。

### <a name="create-saml-sso-for-jira-by-resolution-gmbh-test-user"></a>创建 SAML SSO for Jira by resolution GmbH 测试用户

要使 Azure AD 用户能够登录 SAML SSO for Jira by resolution GmbH，必须将其预配到 SAML SSO for Jira by resolution GmbH 中。  
在 SAML SSO for Jira by resolution GmbH 中，预配是一项手动任务。

**若要预配用户帐户，请执行以下步骤：**

1. 以管理员身份登录到 SAML SSO for Jira by resolution GmbH 公司站点。

2. 将鼠标悬停在小齿轮上，并单击“用户管理”。

    ![添加员工](./media/samlssojira-tutorial/user1.png) 

3. 重定向到“管理员访问权限”页后，输入密码，并单击“确认”按钮。

    ![添加员工](./media/samlssojira-tutorial/user2.png) 

4. 在“用户管理”选项卡部分，单击“创建用户”。

    ![添加员工](./media/samlssojira-tutorial/user3.png) 

5. 在“新建用户”对话框页中，执行以下步骤：

    ![添加员工](./media/samlssojira-tutorial/user4.png) 

    a. 在“电子邮件地址”文本框中，键入用户的电子邮件地址（例如 Brittasimon@contoso.com）。

    b. 在“全名”文本框中，键入用户（例如 Britta Simon）的全名。

    c. 在“用户名”文本框中，键入用户的电子邮件地址（例如 Brittasimon@contoso.com）。

    d. 在“密码”文本框中，键入用户的密码。

    e. 单击“创建用户”。

### <a name="test-single-sign-on"></a>测试单一登录 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的“SAML SSO for Jira by resolution GmbH”磁贴时，应当会自动登录到已为其设置了 SSO 的 SAML SSO for Jira by resolution GmbH。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

