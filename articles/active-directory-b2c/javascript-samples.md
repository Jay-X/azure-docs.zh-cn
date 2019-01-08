---
title: 用于 Azure Active Directory B2C 中的 JavaScript 示例 | Microsoft Docs
description: 了解如何在 Azure Active Directory B2C 中使用 JavaScript。
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4a8d036ff2f36b75df17029c8f00edce25c49e65
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994311"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>用于 Azure Active Directory B2C 中的 JavaScript 示例

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

可以向 Azure Active Directory (Azure AD) B2C 应用程序添加自己的 JavaScript 客户端代码。 本文介绍如何更改[自定义策略](active-directory-b2c-overview-custom.md)以启用脚本执行。

## <a name="prerequisites"></a>先决条件

为应用程序的用户界面元素选择[页协定](page-contract.md)。 如果打算使用 JavaScript，需要为自定义策略中所有的内容定义定义一个页协定版本。

## <a name="add-the-scriptexecution-element"></a>添加 ScriptExecution 元素

通过向 [RelyingParty](relyingparty.md)元素添加 ScriptExecution 元素来启用脚本执行。

1. 打开自定义策略文件。 例如，SignUpOrSignin.xml。
2. 向 RelyingParty 的 UserJourneyBehaviors 元素添加 ScriptExecution 元素：

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. 保存并上传文件。

## <a name="guidelines-for-using-javascript"></a>JavaScript 的使用准则

按照以下准则，使用 JavaScript 自定义应用程序的界面：

- 不对 `<a>` HTML 元素绑定单击事件。 
- 不依赖 Azure AD B2C 代码或注释。
- 不更改 Azure AD B2C HTML 元素的顺序或层次结构。 使用 Azure AD B2C 策略控制 UI 元素的顺序。
- 在调用 RESTful 服务时考虑到以下事项：
    - 可能需要设置 RESTful 服务 CORS 才能实现客户端 HTTP 调用。
    - 确保 RESTful 服务安全，且它仅使用 HTTPS 协议。
    - 不直接使用 JavaScript 来调用Azure AD B2C 终结点。
- 可以嵌入 JavaScript，也可以链接到外部 JavaScript 文件。 如果是使用外部 JavaScript 文件，请确保使用 绝对 URL，而不是相对 URL。
- JavaScript 框架：
    - Azure AD B2C 使用 jQuery 的特定版本。 不包括 jQuery 的其他版本。 在同一页面上使用多个版本会造成问题。
    - 不支持使用 RequireJS。
    - Azure AD B2C 不支持大多数 JavaScript 框架。
- 可通过调用 `window.SETTINGS`、`window.CONTENT` 对象读取 Azure AD B2C 设置，例如当前的 UI 语言。 勿更改这些对象的值。
- 若要自定义 Azure AD B2C 错误消息，请在策略中使用本地化。
- 如果可以通过策略实现一切操作，建议这样做。

## <a name="javascript-samples"></a>JavaScript 示例

### <a name="show-or-hide-a-password"></a>显示或隐藏密码

帮助客户成功注册的一种常见的方式是：允许他们查看他们作为密码输入的内容。 该选项使用户能够根据需要轻松查看和更正密码，从而帮助用户登录。 任何密码类型的字段都有一个带有“显示密码”标签的复选框。  这使用户能够看到纯文本形式的密码。 使自断言页面的注册或登录模板中包括以下代码片段：

```Javascript
function makePwdToggler(pwd){
  // Create show-password checkbox
  var checkbox = document.createElement('input');
  checkbox.setAttribute('type', 'checkbox');
  var id = pwd.id + 'toggler';
  checkbox.setAttribute('id', id);

  var label = document.createElement('label');
  label.setAttribute('for', id);
  label.appendChild(document.createTextNode('show password'));

  var div = document.createElement('div');
  div.appendChild(checkbox);
  div.appendChild(label);

  // Add show-password checkbox under password input
  pwd.insertAdjacentElement('afterend', div);

  // Add toggle password callback
  function toggle(){
    if(pwd.type === 'password'){
      pwd.type = 'text';
    } else {
      pwd.type = 'password';
    }
  }
  checkbox.onclick = toggle;
  // For non-mouse usage
  checkbox.onkeydown = toggle;
}

function setupPwdTogglers(){
  var pwdInputs = document.querySelectorAll('input[type=password]');
  for (var i = 0; i < pwdInputs.length; i++) {
    makePwdToggler(pwdInputs[i]);
  }
}

setupPwdTogglers();
```

### <a name="add-terms-of-use"></a>添加使用条款 

使需要包含“使用条款”复选框的页面中含有以下代码。 本地帐户注册和社交帐户注册页面中通常需要该复选框。  

```Javascript
function addTermsOfUseLink() {
    // find the terms of use label element
    var termsOfUseLabel = document.querySelector('#api label[for="termsOfUse"]');
    if (!termsOfUseLabel) {
        return;
    }
      
    // get the label text
    var termsLabelText = termsOfUseLabel.innerHTML;
      
    // create a new <a> element with the same inner text 
    var termsOfUseUrl = 'https://docs.microsoft.com/legal/termsofuse';
    var termsOfUseLink = document.createElement('a');
    termsOfUseLink.setAttribute('href', termsOfUseUrl);
    termsOfUseLink.setAttribute('target', '_blank');
    termsOfUseLink.appendChild(document.createTextNode(termsLabelText));

    // replace the label text with the new element 
    termsOfUseLabel.replaceChild(termsOfUseLink, termsOfUseLabel.firstChild);
}
```

在代码中，将 `terms-of-use-url` 替换使用条款协议的链接。 为目录创建一个名为“termsOfUse”的属性，然后将它作为用户工作流的用户属性。

## <a name="next-steps"></a>后续步骤

有关如何自定义应用程序用户界面的详细信息，请参阅[在 Azure Active Directory B2C 中使用自定义策略自定义应用程序的用户界面](active-directory-b2c-ui-customization-custom.md)。
