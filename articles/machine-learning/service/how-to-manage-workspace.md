---
title: 创建和管理工作区
titleSuffix: Azure Machine Learning service
description: 了解如何在 Azure 门户中创建、查看和删除 Azure 机器学习服务工作区。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: b65bc1dc510a02144e57e9d057254963d0c398ed
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2019
ms.locfileid: "55244328"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>创建和管理 Azure 机器学习服务工作区

本文将介绍如何在 Azure 门户中针对 [Azure 机器学习服务](overview-what-is-azure-ml.md)创建、查看和删除 [**Azure 机器学习服务工作区**](concept-azure-machine-learning-architecture.md#workspace)。  也可以[使用 CLI](reference-azure-machine-learning-cli.md) 或 [Python 代码](https://aka.ms/aml-sdk)创建和删除工作区。

## <a name="create-a-workspace"></a>创建工作区 

必须有 Azure 订阅，才能创建工作区。 如果还没有 Azure 订阅，请在开始前创建免费帐户。 立即试用 [Azure 机器学习服务免费版或付费版](http://aka.ms/AMLFree)。

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>查看工作区

1. 选择门户左上角的“所有服务”。 

1. 在“所有服务”筛选器字段中，键入“机器学习服务工作区”。  

   ![搜索 Azure 机器学习服务工作区](media/how-to-manage-workspace/allservices-search1.png)

1. 在筛选器结果中，选择“机器学习服务工作区”，以显示工作区列表。 

   ![列出 Azure 机器学习服务工作区](media/how-to-manage-workspace/allservices-search.PNG)

1. 浏览筛选出的工作区列表。 筛选依据可包括订阅、资源组和位置。  

   ![查看工作区](media/how-to-manage-workspace/allservices_view_workspace.PNG)

1. 选择刚刚创建的工作区，即可查看它的属性。

   ![工作区属性](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>创建工作区

单击要删除的工作区顶部的“删除”按钮。

  ![“删除”按钮](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>清理资源 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>后续步骤

学习整个教程，了解如何通过 Azure 机器学习服务使用工作区来生成、定型和部署模型。

> [!div class="nextstepaction"]
> [教程：训练模型](tutorial-train-models-with-aml.md)
