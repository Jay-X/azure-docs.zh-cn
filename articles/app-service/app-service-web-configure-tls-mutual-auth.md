---
title: 配置 TLS 相互身份验证 - Azure 应用服务
description: 了解如何将应用配置为使用 TLS 客户端证书身份验证。
services: app-service
documentationcenter: ''
author: naziml
manager: erikre
editor: jimbe
ms.assetid: cd1d15d3-2d9e-4502-9f11-a306dac4453a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2016
ms.author: naziml
ms.custom: seodec18
ms.openlocfilehash: d441329bc3f279e95b2ee302db53d78f786c3470
ms.sourcegitcommit: e68df5b9c04b11c8f24d616f4e687fe4e773253c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/20/2018
ms.locfileid: "53650391"
---
# <a name="how-to-configure-tls-mutual-authentication-for-azure-app-service"></a>如何为 Azure 应用服务配置 TLS 相互身份验证
## <a name="overview"></a>概述
通过为 Azure 应用服务应用启用不同类型的身份验证可以限制对网站的访问。 执行此操作的方法之一是在通过 TLS/SSL 发送请求时使用客户端证书进行身份验证。 此机制称为 TLS 相互身份验证或客户端证书身份验证，本文将详细说明如何将应用设置为使用客户端证书身份验证。

> **注意：** 如果通过 HTTP 而不是 HTTPS 访问站点，不会收到任何客户端证书。 因此，如果应用程序需要客户端证书，则不应允许通过 HTTP 对应用程序发出请求。
> 
> 

## <a name="configure-app-service-for-client-certificate-authentication"></a>将应用服务配置为使用客户端证书身份验证
要将应用设置为要求使用客户端证书，需要为应用添加 clientCertEnabled 站点设置并将该设置指定为 true。 也可在 SSL 证书边栏选项卡下的 Azure 门户中配置此设置。

可以使用 [ARMClient 工具](https://github.com/projectkudu/ARMClient)轻松创建 REST API 调用。 使用该工具登录之后，将需要发出以下命令：

    ARMClient PUT subscriptions/{Subscription Id}/resourcegroups/{Resource Group Name}/providers/Microsoft.Web/sites/{Website Name}?api-version=2015-04-01 @enableclientcert.json -verbose

将 {} 中的所有内容替换为应用的信息，并创建包含以下 JSON 内容的 enableclientcert.json 文件：

    {
        "location": "My App Location",
        "properties": {
            "clientCertEnabled": true
        }
    }

确保将“location”的值更改为应用所在的位置，例如美国中北部、美国西部等。

还可以使用 https://resources.azure.com 将 `clientCertEnabled` 属性切换为 `true`。

> **注意：** 如果从 Powershell 运行 ARMClient，必须使用重音符 ` 为 JSON 文件转义 \@ 符号。
> 
> 

## <a name="accessing-the-client-certificate-from-app-service"></a>从应用服务访问客户端证书
如果要使用 ASP.NET 并将应用配置为使用客户端证书身份验证，可通过 **HttpRequest.ClientCertificate** 属性使用证书。 对于其他应用程序堆栈，可以通过“X-ARR-ClientCert”请求标头中的 base64 编码值在应用中使用客户端证书。 应用程序可以基于此值创建证书，然后将它用于应用程序中的身份验证和授权。

## <a name="special-considerations-for-certificate-validation"></a>有关证书验证的特殊注意事项
Azure 应用服务平台不会针对发送到应用程序的客户端证书进行任何验证。 验证此证书是应用的责任。 下面是为了进行身份验证而验证证书属性的示例 ASP.NET 代码。

    using System;
    using System.Collections.Specialized;
    using System.Security.Cryptography.X509Certificates;
    using System.Web;

    namespace ClientCertificateUsageSample
    {
        public partial class cert : System.Web.UI.Page
        {
            public string certHeader = "";
            public string errorString = "";
            private X509Certificate2 certificate = null;
            public string certThumbprint = "";
            public string certSubject = "";
            public string certIssuer = "";
            public string certSignatureAlg = "";
            public string certIssueDate = "";
            public string certExpiryDate = "";
            public bool isValidCert = false;

            //
            // Read the certificate from the header into an X509Certificate2 object
            // Display properties of the certificate on the page
            //
            protected void Page_Load(object sender, EventArgs e)
            {
                NameValueCollection headers = base.Request.Headers;
                certHeader = headers["X-ARR-ClientCert"];
                if (!String.IsNullOrEmpty(certHeader))
                {
                    try
                    {
                        byte[] clientCertBytes = Convert.FromBase64String(certHeader);
                        certificate = new X509Certificate2(clientCertBytes);
                        certSubject = certificate.Subject;
                        certIssuer = certificate.Issuer;
                        certThumbprint = certificate.Thumbprint;
                        certSignatureAlg = certificate.SignatureAlgorithm.FriendlyName;
                        certIssueDate = certificate.NotBefore.ToShortDateString() + " " + certificate.NotBefore.ToShortTimeString();
                        certExpiryDate = certificate.NotAfter.ToShortDateString() + " " + certificate.NotAfter.ToShortTimeString();
                    }
                    catch (Exception ex)
                    {
                        errorString = ex.ToString();
                    }
                    finally 
                    {
                        isValidCert = IsValidClientCertificate();
                        if (!isValidCert) Response.StatusCode = 403;
                        else Response.StatusCode = 200;
                    }
                }
                else
                {
                    certHeader = "";
                }
            }

            //
            // This is a SAMPLE verification routine. Depending on your application logic and security requirements, 
            // you should modify this method
            //
            private bool IsValidClientCertificate()
            {
                // In this example we will only accept the certificate as a valid certificate if all the conditions below are met:
                // 1. The certificate is not expired and is active for the current time on server.
                // 2. The subject name of the certificate has the common name nildevecc
                // 3. The issuer name of the certificate has the common name nildevecc and organization name Microsoft Corp
                // 4. The thumbprint of the certificate is 30757A2E831977D8BD9C8496E4C99AB26CB9622B
                //
                // This example does NOT test that this certificate is chained to a Trusted Root Authority (or revoked) on the server 
                // and it allows for self signed certificates
                //

                if (certificate == null || !String.IsNullOrEmpty(errorString)) return false;

                // 1. Check time validity of certificate
                if (DateTime.Compare(DateTime.Now, certificate.NotBefore) < 0 || DateTime.Compare(DateTime.Now, certificate.NotAfter) > 0) return false;

                // 2. Check subject name of certificate
                bool foundSubject = false;
                string[] certSubjectData = certificate.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certSubjectData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundSubject = true;
                        break;
                    }
                }
                if (!foundSubject) return false;

                // 3. Check issuer name of certificate
                bool foundIssuerCN = false, foundIssuerO = false;
                string[] certIssuerData = certificate.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
                foreach (string s in certIssuerData)
                {
                    if (String.Compare(s.Trim(), "CN=nildevecc") == 0)
                    {
                        foundIssuerCN = true;
                        if (foundIssuerO) break;
                    }

                    if (String.Compare(s.Trim(), "O=Microsoft Corp") == 0)
                    {
                        foundIssuerO = true;
                        if (foundIssuerCN) break;
                    }
                }

                if (!foundIssuerCN || !foundIssuerO) return false;

                // 4. Check thumprint of certificate
                if (String.Compare(certificate.Thumbprint.Trim().ToUpper(), "30757A2E831977D8BD9C8496E4C99AB26CB9622B") != 0) return false;

                // If you also want to test if the certificate chains to a Trusted Root Authority you can uncomment the code below
                //
                //X509Chain certChain = new X509Chain();
                //certChain.Build(certificate);
                //bool isValidCertChain = true;
                //foreach (X509ChainElement chElement in certChain.ChainElements)
                //{
                //    if (!chElement.Certificate.Verify())
                //    {
                //        isValidCertChain = false;
                //        break;
                //    }
                //}
                //if (!isValidCertChain) return false;

                return true;
            }
        }
    }
