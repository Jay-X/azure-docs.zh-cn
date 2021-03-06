---
title: Azure Stack 验证最佳做法 |Microsoft Docs
description: 本文介绍使用验证作为服务的最佳实践。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 11/26/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 415cdecc33b7360d482d37a3cb9d4f1bce528ab1
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251786"
---
# <a name="create-an-oem-package"></a>创建一个 OEM 包

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Azure Stack OEM 扩展包是由哪个 OEM 特定的内容添加到 Azure Stack 基础结构，用于在部署和操作的进程，如 update、 扩展以及现场更换的机制。

## <a name="creating-the-package"></a>创建包

创建并验证后，可以在 VaaS 使用 OEM 扩展包。  在继续之前，确保已完成的步骤[创建一个 OEM 包](https://microsoft.sharepoint.com/:w:/r/teams/cloudsolutions/Sacramento/_layouts/15/Doc.aspx?sourcedoc=%7BD7406069-7661-419C-B3B1-B6A727AB3972%7D&file=Azure%20Stack%20OEM%20Extension%20Package.docx&action=default&mobileredirect=true)。 然后将包提交给 Microsoft 以及以解决方案验证工作流中的登录 VaaS 测试结果。 以下步骤详细说明了如何绑定到单个 zip 文件 VaaS 可使用生成的文件。

1. 标识包的以下内容：
    - 名为可执行文件 `<Publisher>-<Model>-<Version>.exe`
    - 一个或多个文件必须命名为二进制文件`<Publisher><Model>-<Version>-#.bin`，其中 # 是从 1 开始的序号。 二进制文件数为依赖于包内容的总大小。
    - 名为的清单文件`oemMetadata.xml`，应将包内容的根目录中的 metadata.xml 文件的内容相同。

2. 选择的内容文件和内容创建的 zip 文件：

    ![Zip 文件内容](media/vaas-create-oem-package-1.png)![压缩项内容](media/vaas-create-oem-package-2.png)

3. 重命名生成的文件，因此它是描述性，以便识别它。

## <a name="verifying-the-contents"></a>验证内容

若要验证你的 zip 文件的结构，检查它，并请检查有任何子文件夹。 TLD 已压缩的内容。 有效的包结构如下所示。
> [!IMPORTANT]
> 压缩而不是内容的父文件夹将导致包签名失败。

![正确压缩的包内容](media/vaas-create-oem-package-3.png)

可上传到 VaaS 和由 Microsoft 签名解决方案验证工作流中的 zip 文件。

## <a name="next-steps"></a>后续步骤

- [验证 OEM 包](azure-stack-vaas-validate-oem-package.md)
