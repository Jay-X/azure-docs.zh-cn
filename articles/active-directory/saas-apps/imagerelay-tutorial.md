---
title: 教程：Azure Active Directory 与 Image Relay 集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Image Relay 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 65bb5990-07ef-4244-9f41-cd28fc2cb5a2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: 4bc522915b302c7875ca77213a39fce52f0736a1
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2019
ms.locfileid: "55162236"
---
# <a name="tutorial-azure-active-directory-integration-with-image-relay"></a>教程：Azure Active Directory 与 Image Relay 集成

在本教程中，了解如何将 Image Relay 与 Azure Active Directory (Azure AD) 集成。

将 Image Relay 与 Azure AD 集成可提供以下优势：

- 可在 Azure AD 中控制谁有权访问 Image Relay
- 可以让用户使用其 Azure AD 帐户自动登录到 Image Relay（单一登录）
- 可以在一个中心位置（即 Azure 门户）管理帐户

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 Image Relay 的集成，需要以下项：

- Azure AD 订阅
- 已启用 Image Relay 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以在[此处](https://azure.microsoft.com/pricing/free-trial/)获取一个月的试用版。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 Image Relay
1. 配置和测试 Azure AD 单一登录

## <a name="adding-image-relay-from-the-gallery"></a>从库中添加 Image Relay
要配置 Image Relay 与 Azure AD 的集成，需要从库中将 Image Relay 添加到托管 SaaS 应用列表。

**若要从库添加 Image Relay，请按以下步骤操作：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![Active Directory][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![应用程序][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![应用程序][3]

1. 在搜索框中，键入“Image Relay”。

    ![创建 Azure AD 测试用户](./media/imagerelay-tutorial/tutorial_imagerelay_search.png)

1. 在结果窗格中，选择“Image Relay”，然后单击“添加”按钮添加该应用程序。

    ![创建 Azure AD 测试用户](./media/imagerelay-tutorial/tutorial_imagerelay_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录
在本部分中，将基于名为“Britta Simon”的测试用户配置和测试 Image Relay 的 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 Image Relay 用户。 换句话说，需要建立 Azure AD 用户与 Image Relay 中相关用户之间的链接关系。

可通过将 Azure AD 中“用户名”的值指定为 Image Relay 中“用户名”的值来建立此链接关系。

若要配置和测试 Image Relay 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configuring-azure-ad-single-sign-on)** - 让用户使用此功能。
1. **[创建 Azure AD 测试用户](#creating-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 Image Relay 测试用户](#creating-an-image-relay-test-user)** - 在 Image Relay 中创建 Britta Simon 的对应用户，并将其链接到该用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assigning-the-azure-ad-test-user)** - 让 Britta Simon 使用 Azure AD 单一登录。
1. **[测试单一登录](#testing-single-sign-on)** - 验证配置是否正常工作。

### <a name="configuring-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 Image Relay 应用程序中配置单一登录。

**若要配置 Image Relay 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 Image Relay 应用程序集成页上，单击“单一登录”。

    ![配置单一登录][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。
 
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_samlbase.png)

1. 在“Image Relay 域和 URL”部分中，执行以下步骤：

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_url.png)

    a. 在“登录 URL”文本框中，使用以下模式键入 URL： `https://<companyname>.imagerelay.com/`

    b. 在“标识符”文本框中，使用以下模式键入 URL：`https://<companyname>.imagerelay.com/sso/metadata`

    > [!NOTE] 
    > 这些不是实际值。 必须使用实际登录 URL 和标识符更新这些值。 请联系 [Image Relay 客户端支持团队](http://support.imagerelay.com/)获取这些值。 
 


1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_general_400.png)

1. 在“Image Relay 配置”部分，单击“配置 Image Relay”打开“配置登录”窗口。 从“快速参考”部分中复制“注销服务 URL 和 SAML 单一登录服务 URL”。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_configure.png) 

1. 在另一个浏览器窗口中，以管理员身份登录到 Image Relay 公司站点。

1. 在顶部工具栏中，单击“用户和权限”工作负荷。
   
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_06.png) 

1. 单击“创建新权限”。
   
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_08.png)

1. 在“单一登录设置”工作负荷中，选中“此组只能通过单一登录进行登录”复选框，并单击“保存”。
   
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_09.png) 

1. 转到“帐户设置”。
   
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_10.png) 

1. 转到“单一登录设置”工作负荷。
    
     ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_11.png)

1. 在“SAML 设置”对话框中，执行以下步骤：
    
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_12.png)
    
    a. 在“登录 URL”文本框中，粘贴从 Azure 门户复制的“单一登录服务 URL”值。

    b. 在“注销 URL”文本框中，粘贴从 Azure 门户复制的“单一注销服务 URL”值。

    c. 对于“名称 ID 格式”，选择“urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress”。

    d. 对于“来自服务提供商(Image Relay)的请求的绑定选项”，选择“POST 绑定”。

    e. 在“x.509 证书”下，单击“更新证书”。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_17.png)

    f. 在记事本中打开下载的证书，复制其内容，然后将其粘贴到“x.509 证书”文本框中。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_18.png)

    g. 在“实时用户预配”部分中，选择“启用实时用户预配”。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_19.png)

    h. 选择仅允许通过单一登录进行登录的权限组（例如，**SSO 基本**）。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_20.png)

    i. 单击“ **保存**”。

> [!TIP]
> 之后在设置应用时，就可以在 [Azure 门户](https://portal.azure.com)中阅读这些说明的简明版本了！  从“Active Directory”>“企业应用程序”部分添加此应用后，只需单击“单一登录”选项卡，即可通过底部的“配置”部分访问嵌入式文档。 可在此处阅读有关嵌入式文档功能的详细信息：[Azure AD 嵌入式文档]( https://go.microsoft.com/fwlink/?linkid=845985)


### <a name="creating-an-azure-ad-test-user"></a>创建 Azure AD 测试用户
本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

![创建 Azure AD 用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 **Azure 门户**的左侧导航窗格中，单击“Azure Active Directory”图标。

    ![创建 Azure AD 测试用户](./media/imagerelay-tutorial/create_aaduser_01.png) 

1. 若要显示用户列表，请转到“用户和组”，单击“所有用户”。
    
    ![创建 Azure AD 测试用户](./media/imagerelay-tutorial/create_aaduser_02.png) 

1. 若要打开“用户”对话框，请在对话框顶部单击“添加”。
 
    ![创建 Azure AD 测试用户](./media/imagerelay-tutorial/create_aaduser_03.png) 

1. 在“用户”对话框页上，执行以下步骤：
 
    ![创建 Azure AD 测试用户](./media/imagerelay-tutorial/create_aaduser_04.png) 

    a. 在“名称”文本框中，键入 **BrittaSimon**。

    b. 在“用户名”文本框中，键入 BrittaSimon 的“电子邮件地址”。

    c. 选择“显示密码”并记下“密码”的值。

    d. 单击“创建”。
 
### <a name="creating-an-image-relay-test-user"></a>创建 Image Relay 测试用户

本部分的目的是在 Image Relay 中创建名为“Britta Simon”的用户。

**若要在 Image Relay 中创建名为“Britta Simon”的用户，请执行以下步骤：**

1. 以管理员身份登录 Image Relay 公司站点。

1. 转到“用户和权限”，并选择“创建 SSO 用户”。
   
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_21.png) 

1. 输入要预配的用户的“电子邮件”、“名字”、“姓氏”和“公司”，并选择只能通过单一登录进行登录的权限组（例如，SSO 基本）。
   
    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_22.png) 

1. 单击“创建”。

### <a name="assigning-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 Britta Simon 访问 Image Relay 的权限，允许其使用 Azure 单一登录。

![分配用户][200] 

**要将 Britta Simon 分配到 Image Relay，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“Image Relay”。

    ![配置单一登录](./media/imagerelay-tutorial/tutorial_imagerelay_app.png) 

1. 在左侧菜单中，单击“用户和组”。

    ![分配用户][202] 

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![分配用户][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="testing-single-sign-on"></a>测试单一登录

本部分旨在使用“访问面板”测试 Azure AD 单一登录配置。    

单击访问面板中的 Image Relay 磁贴时，应当会自动登录到 Image Relay 应用程序。


## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [Azure Active Directory 的应用程序访问与单一登录是什么？](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/imagerelay-tutorial/tutorial_general_01.png
[2]: ./media/imagerelay-tutorial/tutorial_general_02.png
[3]: ./media/imagerelay-tutorial/tutorial_general_03.png
[4]: ./media/imagerelay-tutorial/tutorial_general_04.png


[100]: ./media/imagerelay-tutorial/tutorial_general_100.png

[200]: ./media/imagerelay-tutorial/tutorial_general_200.png
[201]: ./media/imagerelay-tutorial/tutorial_general_201.png
[202]: ./media/imagerelay-tutorial/tutorial_general_202.png
[203]: ./media/imagerelay-tutorial/tutorial_general_203.png

