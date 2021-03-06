---
title: 在 Azure IoT Central 应用程序中创建和管理事件规则 | Microsoft Docs
description: 可以通过 Azure IoT Central 事件规则以近实时方式监视设备并自动调用操作（例如在触发规则时发送电子邮件）。
author: ankitscribbles
ms.author: ankitgup
ms.date: 02/02/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 8655265f5f793741c2d563d1e79d4565700e0128
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2019
ms.locfileid: "55768511"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>在 Azure IoT Central 应用程序中创建事件规则并设置通知

*本文适用于操作员、构建者和管理员。*

可以使用 Azure IoT Central 对连接的设备进行远程监视。 可以通过 Azure IoT Central 规则近乎实时地监视设备并自动调用操作（例如发送电子邮件或触发 Microsoft Flow）。 只需几次单击即可定义用于监视设备数据的条件并配置相应操作。 本文介绍如何创建规则来监视设备发送的事件。

设备可以使用事件度量来发送重要或信息性设备事件。 当设备报告所选设备事件时，会触发事件规则。

## <a name="create-an-event-rule"></a>创建事件中心

若要创建事件规则，设备模板必须至少定义有一个事件度量。 此示例使用冷藏食品贩卖机设备来报告风扇电机错误事件。 此规则监视由设备报告的事件，只要报告了事件，就会发送电子邮件。

1. 使用 Device Explorer，导航到要对其添加规则的设备模板。

1. 在所选模板下，单击一个现有设备。

    >[!TIP] 
    >如果此模板无任何设备，请先添加一个新设备。

1. 如果尚未创建任何规则，则会看到以下屏幕：

    ![尚无规则](media/howto-create-event-rules-experimental/Rules_Landing_Page.png)


1. 在“规则”选项卡上，依次单击“编辑模板”和“+ 新建规则”，即可查看可以创建的规则类型。


1. 单击“事件”磁贴创建事件监视规则。

    ![规则类型](media/howto-create-event-rules-experimental/Rule_Types.png)

    
1. 输入一个有助于在此设备模板中识别该规则的名称。

1. 若要立即为所有根据此模板创建的设备启用此规则，请将“为此模板的所有设备启用规则”切换为开启状态。

    ![规则详细信息](media/howto-create-event-rules-experimental/Rule_Detail.png)

    此规则自动应用到该设备模板下的所有设备。

### <a name="configure-the-rule-conditions"></a>配置规则条件

条件用于定义此规则监视的条件。

1. 选择“条件”旁边的“+”，添加新的条件。

1. 从“度量”下拉列表中选择要监视的事件。 在此示例中，已选择“风扇电机错误”事件。

   ![条件](media/howto-create-event-rules-experimental/Condition_Filled_Out.png)


1. 或者，还可将“计数”设为“聚合”，并提供相应阈值。

    - 如无聚合，此规则对每个满足此条件的事件数据点均会触发。 例如，如果将此规则的条件配置为在“风扇电机错误”事件发生时触发，则设备报告该事件时会立即触发此规则。
    - 如果“计数”用作聚合函数，则必须提供“阈值”和“聚合时间段”（此期间内需评估条件）。 在这种情况下，事件计数会聚合，且此规则仅在聚合事件计数匹配阈值时触发。
 
    例如，如果要对 5 分钟内存在超过 3 个设备事件的情况发出警报，请选择事件，然后将聚合函数设为“计数”，将运算符设为“大于”，并将“阈值”设为“3”。 将“聚合时间段”设为“5 分钟”。 设备在 5 分钟内发送超过 3 个事件时，此规则会触发。 规则评估频率与“聚合时间段”相同，也就是说，此例中规则每隔 5 分钟评估一次。 

    ![添加事件条件](media/howto-create-event-rules-experimental/Aggregate_Condition_Filled_Out.png)

    >[!NOTE] 
    >“条件”下可添加多个事件度量。 如果指定多个条件，必须满足所有条件才能触发该规则。 每个条件通过“AND”子句隐式联接。 使用聚合时，必须聚合每个度量。

### <a name="configure-actions"></a>配置操作

本部分演示如何设置在规则触发时要执行的操作。 此规则中指定的所有条件评估结果均为 true 时会调用操作。

1. 选择“操作”旁边的“+”。 在此处可以看到可用操作的列表。

    ![添加操作](media/howto-create-event-rules-experimental/Add_Action.png)

1. 选择“电子邮件”操作，在“收件人”字段中输入有效的电子邮件地址，并提供一个说明，用于在触发规则时显示在电子邮件的正文中。

    > [!NOTE]
    > 电子邮件只发送给那些已添加到应用程序中并已至少登录一次的用户。 详细了解 Azure IoT Central 中的[用户管理](howto-administer-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)。

   ![配置操作](media/howto-create-event-rules-experimental/Configure_Action.png)

1. 若要保存规则，请选择“保存”。 此规则数分钟即可生效，然后就开始监视发送到应用程序的事件。 匹配规则中指定的条件时，规则会触发配置的电子邮件操作。

可将其他操作添加到规则，例如 Microsoft Flow 和 Webhook。 每个规则最多可添加 5 个操作。

- [Microsoft Flow 操作](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)，在触发规则时启动 Microsoft Flow 中的工作流 
- [Webhook 操作](howto-create-webhooks-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)，在触发规则时通知其他服务

## <a name="parameterize-the-rule"></a>将规则参数化

也可使用“设备属性”作为参数来配置操作。 如果将电子邮件地址存储为设备属性，则可在定义“收件人”地址时使用它。

## <a name="delete-a-rule"></a>删除规则

如果不再需要某项规则，可打开该规则并选择“删除”，以将其删除。 删除规则会将其从设备模板和所有关联的设备中删除。

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>启用或禁用设备模板的规则

导航到设备，然后选择要启用或禁用的规则。 切换“为此模板的所有设备启用规则”按钮可为与设备模板关联的所有设备启用或禁用该规则。

## <a name="enable-or-disable-a-rule-for-a-device"></a>启用或禁用设备的规则

导航到设备，然后选择要启用或禁用的规则。 切换“启用此设备的规则”按钮可启用或禁用该设备的规则。

## <a name="next-steps"></a>后续步骤

了解如何在 Azure IoT Central 应用程序中创建规则后，可了解如下后续步骤：

- [在规则中添加 Microsoft Flow 操作](howto-add-microsoft-flow-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [在规则中添加 Webhook 操作](howto-create-webhooks-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [如何管理设备](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
