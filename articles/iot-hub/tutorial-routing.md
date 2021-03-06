---
title: 使用 Azure IoT 中心 (.NET) 配置消息路由 | Microsoft Docs
description: 使用 Azure IoT 中心配置消息路由
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 09/11/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 22678a44cda38b52982e977281d3310efde15831
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/12/2019
ms.locfileid: "54247241"
---
# <a name="tutorial-configure-message-routing-with-iot-hub"></a>教程：使用 IoT 中心配置消息路由

[消息路由](iot-hub-devguide-messages-d2c.md)能够将遥测数据从 IoT 设备发送到内置的与事件中心兼容的终结点或自定义终结点，例如，blob 存储、服务总线队列、服务总线主题和事件中心。 配置消息路由时，可以创建[路由查询](iot-hub-devguide-routing-query-syntax.md)来自定义符合特定条件的路由。 设置完成后，引入的数据将通过 IoT 中心自动路由到终结点。 

在本教程中，可了解如何通过 IoT 中心设置和使用路由查询。 将消息从 IoT 设备路由到多项服务（包括 blob 存储和服务总线队列）的其中一项服务中。 路由到服务总线队列的消息将有逻辑应用获取并通过电子邮件发送。 没有专门设置路由的消息将发送到默认终结点，可在 Power BI 可视化中查看。

将在本教程中执行以下任务：

> [!div class="checklist"]
> * 使用 Azure CLI 或 PowerShell 设置基础资源 - IoT 中心、存储帐户、服务总线队列和模拟设备。
> * 在 IoT 中心为存储帐户和服务总线队列配置终结点和路由。
> * 创建一个逻辑应用，该应用将在消息添加到服务总线队列时触发，并发送电子邮件。
> * 下载并运行应用，该应用模拟 IoT 设备将消息发送到中心，以获得不同的路由选择。
> * 为发送至默认终结点的数据创建 Power BI 可视化。
> * 查看结果...
> * ...在服务总线队列和电子邮件中。
> * ...在存储帐户中。
> * ...在 Power BI 可视化中。

## <a name="prerequisites"></a>先决条件

- Azure 订阅。 如果没有 Azure 订阅，请在开始之前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

- 安装 [Visual Studio](https://www.visualstudio.com/)。 

- 用于分析默认终结点的流分析的 Power BI 帐户。 （[免费试用 Power BI](https://app.powerbi.com/signupredirect?pbi_source=web)。）

- 用于发送通知电子邮件的 Office 365 帐户。 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="set-up-resources"></a>设置资源

要完成本教程，需要 IoT 中心、存储帐户和服务总线队列。 这些资源可以通过 Azure CLI 或 Azure PowerShell 创建。 为所有资源使用相同的资源组和位置。 然后在结束时，可通过删除资源组一步删除所有内容。

以下部分介绍如何执行所需步骤。 按照 CLI 或 PowerShell 说明操作。

1. 创建[资源组](../azure-resource-manager/resource-group-overview.md)。 

2. 在 S1 层级中创建 IoT 中心。 将使用者组添加到 IoT 中心。 检索数据时，Azure 流分析使用使用者组。

   > [!NOTE]
   > 必须使用付费层中的 IoT 中心来完成本教程。 免费层只允许设置一个终结点，但本教程需要多个终结点。
   > 

3. 使用 Standard_LRS 副本创建标准 V1 存储帐户。

4. 创建服务总线命名空间和队列。 

5. 为发送消息到中心的模拟设备创建设备标识。 保存测试阶段的密钥。

### <a name="set-up-your-resources-using-azure-cli"></a>使用 Azure CLI 设置资源

复制此脚本并将其粘贴到 Cloud Shell。 在已登录的情况下，该脚本会逐行运行。 

必须全局唯一的变量已使用 `$RANDOM` 串联起来。 运行脚本时，如果已设置相应的变量，则会生成一个随机数字字符串，并将其串联到固定字符串的末尾，使其保持唯一。

```azurecli-interactive

# This is the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device 

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, so add a random number to the end.
iotHubName=ContosoTestHub$RANDOM
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, so add a random number to the end.
storageAccountName=contosostorage$RANDOM
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "$storageAccountKey"

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off 

# The Service Bus namespace must be globally unique, so add a random number to the end.
sbNameSpace=ContosoSBNamespace$RANDOM
echo "Service Bus namespace = " $sbNameSpace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNameSpace \
    --location $location
    
# The Service Bus queue name must be globally unique, so add a random number to the end.
sbQueueName=ContosoSBQueue$RANDOM
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNameSpace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

```

### <a name="set-up-your-resources-using-azure-powershell"></a>使用 Azure PowerShell 设置资源

复制此脚本并将其粘贴到 Cloud Shell。 在已登录的情况下，该脚本会逐行运行。

必须全局唯一的变量已使用 `$(Get-Random)` 串联起来。 运行脚本时，如果已设置相应的变量，则会生成一个随机数字字符串，并将其串联到固定字符串的末尾，使其保持唯一。

```azurepowershell-interactive
# Log into Azure account.
Login-AzureRMAccount

# Set the values for the resource names that don't have to be globally unique.
# The resources that have to have unique names are named in the script below
#   with a random number concatenated to the name so you can probably just
#   run this script, and it will work with no conflicts.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"
$iotDeviceName = "Contoso-Test-Device"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzureRmResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, so add a random number to the end.
$iotHubName = "ContosoTestHub$(Get-Random)"
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzureRmIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzureRmIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random number to the end.
$storageAccountName = "contosostorage$(Get-Random)"
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
$storageContext = $storageAccount.Context 

# Create the container in the storage account.
New-AzureStorageContainer -Name $containerName `
    -Context $storageContext

# The Service Bus namespace must be globally unique,
#   so add a random number to the end.
$serviceBusNamespace = "ContosoSBNamespace$(Get-Random)"
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzureRmServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random number to the end.
$serviceBusQueueName  = "ContosoSBQueue$(Get-Random)"
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzureRmServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName 

```

接下来，创建设备标识并保存其密钥供之后使用。 此设备标识由模拟应用程序用来发送消息到 IoT 中心。 PowerShell 不提供此功能，但可在 [Azure 门户](https://portal.azure.com)中创建设备。

1. 打开 [Azure 门户](https://portal.azure.com)并登录到 Azure 帐户。

2. 单击“资源组”并选择相应资源组。 本教程使用 ContosoResources。

3. 在资源列表中，单击你的 IoT 中心。 本教程使用 ContosoTestHub。 从中心窗格选择“IoT 设备”。

4. 单击“+ 添加”。 在添加设备窗格中，填写设备 ID。 本教程使用 Contoso-Test-Device。 将密钥留空，勾选“自动生成密钥”。 确保已启用“将设备连接到 IoT 中心”。 单击“ **保存**”。

   ![显示添加设备屏幕的屏幕截图。](./media/tutorial-routing/add-device.png)

5. 现在已创建设备，单击它可显示生成的密钥。 单击主密钥上的“复制”图标，将其保存在某个位置（如记事本）供本教程的测试阶段使用。

   ![显示设备详细信息（包括密钥）的屏幕截图。](./media/tutorial-routing/device-details.png)

## <a name="set-up-message-routing"></a>设置消息路由

根据模拟设备附加到消息的属性将消息路由到不同资源。 未自定义路由的消息将发送到默认终结点（消息/事件）。 

|值 |结果|
|------|------|
|级别=“storage” |写入到 Azure 存储。|
|级别=“critical” |写入服务总线队列。 逻辑应用从队列检索消息并使用 Office 365 通过电子邮件发送该消息。|
|default |使用 Power BI 显示此数据。|

### <a name="routing-to-a-storage-account"></a>路由到存储帐户 

现在为存储帐户设置路由。 你转到“消息路由”窗格，然后添加路由。 添加路由时，请为路由定义新的终结点。 此设置完成后，“级别”属性设置为“storage”的消息将自动写入存储帐户。 

数据以 Avro 格式写入 Blob 存储。

1. 在 [Azure 门户](https://portal.azure.com)中，单击“资源组”，然后选择你的资源组。 本教程使用 ContosoResources。 

2. 在资源列表下单击 IoT 中心。 本教程使用 ContosoTestHub。 

3. 单击“消息路由”。 在“消息路由”窗格中，单击“+添加”。 在“添加路由”窗格中，单击终结点字段旁边的“+添加”，如下图所示：

   ![显示如何开始将终结点添加到路由的屏幕截图。](./media/tutorial-routing/message-routing-add-a-route-w-storage-ep.png)

4. 选择“Blob 存储”。 随即看到“添加存储终结点”窗格。 

   ![显示添加终结点的屏幕截图。](./media/tutorial-routing/message-routing-add-storage-ep.png)

5. 为终结点输入名称。 本教程使用 storageContainer。

6. 单击“选取容器”。 将转到存储帐户列表。 选择在准备步骤中设置的存储账户。 本教程使用 **contosostorage**。 它显示该存储帐户中的容器列表。 选择在准备步骤中设置的容器。 本教程使用 contosoresults。 单击“选择”。 随即返回到“添加终结点”窗格。 

7. 在本教程中，其余字段使用默认值。 

   > [!NOTE]
   > 可以使用 **Blob 文件名格式**设置 Blob 名称的格式。 默认为 `{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}`。 格式必须包含 {iothub}、{partition}、{YYYY}、{MM}、{DD}、{HH} 和 {mm}，顺序不限。 
   > 
   > 例如，使用默认 Blob 文件名格式时，如果中心名称为 ContosoTestHub，日期/时间为 2018 年 10 月 30 日上午 10:56，则 Blob 名称将类似于：`ContosoTestHub/0/2018/10/30/10/56`。
   > 
   > Blob 以 Avro 格式写入。
   >

8. 单击“创建”以创建存储终结点并将其添加到路由。 随即返回到“添加路由”窗格。

9. 现在完成余下的路由查询信息。 此查询指定将消息发送到刚刚添加为终结点的存储容器的条件。 填充屏幕上的字段。 

   **名称**：为路由查询输入名称。 本教程使用 StorageRoute。

   **终结点**：选择刚刚设置的终结点。 
   
   **数据源**：从下拉列表选择“设备遥测消息”。

   **启用路由**：确保启用此选项。
   
   **路由查询**：输入 `level="storage"` 作为查询字符串。 

   ![显示为存储帐户创建路由查询的屏幕截图。](./media/tutorial-routing/message-routing-finish-route-storage-ep.png)  
   
   单击“ **保存**”。 完成后，返回到“消息路由”窗格，可在其中看到存储的新路由查询。 关闭“路由”窗格，将返回到资源组页。

### <a name="routing-to-a-service-bus-queue"></a>路由到服务总线队列 

现在为服务总线队列设置路由。 你转到“消息路由”窗格，然后添加路由。 添加路由时，请为路由定义新的终结点。 此设置完成后，“级别”属性设置为“critical”的消息将写入服务总线队列，这样会触发逻辑应用，然后发送具有此信息的电子邮件。 

1. 在资源组页上，单击 IoT 中心，然后单击“消息路由”。 

2. 在“消息路由”窗格中，单击“+添加”。 

3. 在“添加路由”窗格中，单击终结点字段旁边的“+添加”。 选择“服务总线队列”。 随即看到“添加服务总线终结点”窗格。 

   ![显示添加服务总线终结点的屏幕截图](./media/tutorial-routing/message-routing-add-sbqueue-ep.png)

4. 填写字段：

   **终结点名称**：为终结点输入名称。 本教程使用 CriticalQueue。
   
   **服务总线命名空间**：单击此字段以显示下拉列表；选择你在准备步骤中设置的服务总线命名空间。 本教程使用 ContosoSBNamespace。

   **服务总线队列**：单击此字段以显示下拉列表；从下拉列表中选择服务总线队列。 本教程使用 contososbqueue。

5. 单击“创建”添加服务总线队列终结点。 随即返回到“添加路由”窗格。 

6.  现在完成余下的路由查询信息。 此查询指定将消息发送到刚刚添加为终结点的服务总线队列的条件。 填充屏幕上的字段。 

   **名称**：为路由查询输入名称。 本教程使用 SBQueueRoute。 

   **终结点**：选择刚刚设置的终结点。

   **数据源**：从下拉列表选择“设备遥测消息”。

   **路由查询**：输入 `level="critical"` 作为查询字符串。 

   ![显示为服务总线队列创建路由查询的屏幕截图。](./media/tutorial-routing/message-routing-finish-route-sbq-ep.png)

7. 单击“ **保存**”。 返回到“路由”窗格时，可看到这两个新的路由，如此处所示。

   ![显示刚设置的路由的屏幕截图。](./media/tutorial-routing/message-routing-show-both-routes.png)

8. 可以通过单击“自定义终结点”选项卡查看你设置的自定义端点。

   ![显示刚刚设置的自定义终结点的屏幕截图。](./media/tutorial-routing/message-routing-show-custom-endpoints.png)

9. 关闭“消息路由”窗格，返回到“资源组”窗格。

## <a name="create-a-logic-app"></a>创建逻辑应用  

服务总线队列将用于检索指定为关键的消息。 设置一个逻辑应用，用于监视服务总线队列，并在消息添加到队列时发送电子邮件。 

1. 在 [Azure 门户](https://portal.azure.com)中，单击“+创建资源”。 在搜索框中输入“逻辑应用”，并单击 Enter。 从显示的搜索结果中，选择逻辑应用，然后单击“创建”，进入“创建逻辑应用”窗格。 填充字段。 

   **名称**：此字段是逻辑应用的名称。 本教程使用 ContosoLogicApp。 

   **订阅**：选择 Azure 订阅。

   **资源组**：单击“使用现有资源组”，然后选择你的资源组。 本教程使用 ContosoResources。 

   **位置**：使用你的位置。 本教程使用“美国西部”。 

   **Log Analytics**：应关闭此开关。 

   ![显示“创建逻辑应用”屏幕的屏幕截图。](./media/tutorial-routing/create-logic-app.png)

   单击“创建”。

2. 现在转到该逻辑应用。 要转到逻辑应用，最简单方法是单击“资源组”，选择你的资源组（本教程使用 ContosoResources），然后从资源列表选择该逻辑应用。 随即将显示该逻辑应用设计器页面（可能需要向右滚动才可查看完整页面）。 在该逻辑应用设计器页面上，向下滚动，直到看见显示“空白的逻辑应用+”的磁贴，单击该磁贴。 

3. 此时会显示连接器列表。 选择“服务总线”。 

   ![显示连接器列表的屏幕截图。](./media/tutorial-routing/logic-app-connectors.png)

4. 此时会显示触发器的列表。 选择“服务总线 - 在队列中收到邮件时(自动完成)”。 

   ![显示服务总线的触发器列表的屏幕截图。](./media/tutorial-routing/logic-app-triggers.png)

5. 在下一屏幕上，填写“连接名称”。 本教程使用 ContosoConnection。 

   ![显示为服务总线队列设置连接的屏幕截图。](./media/tutorial-routing/logic-app-define-connection.png)

   单击“服务总线”命名空间。 本教程使用 ContosoSBNamespace。 选择该命名空间时，门户会查询该“服务总线”命名空间以检索密钥。 选择“RootManageSharedAccessKey”，并单击“创建”。 
   
   ![显示完成设置连接的屏幕截图。](./media/tutorial-routing/logic-app-finish-connection.png)

6. 在下一步屏幕上，从下拉列表选择队列名称（本教程使用 contososbqueue）。 其余字段可使用默认值。 

   ![显示队列选项的屏幕截图。](./media/tutorial-routing/logic-app-queue-options.png)

7. 现在，设置一个在队列接收到消息时发送电子邮件的操作。 在逻辑应用设计器中，单击“+新建步骤”添加步骤，然后单击“添加操作”。 在“选择操作”窗格中，找到并单击“Office 365 Outlook”。 在触发器屏幕上，选择“Office 365 Outlook - 发送电子邮件”。  

   ![显示 Office365 选项的屏幕截图。](./media/tutorial-routing/logic-app-select-outlook.png)

8. 接下来，登录到 Office 365 帐户，设置连接。 为电子邮件收件人指定电子邮件地址。 同时指定主题，并在正文键入想要让收件人看到的消息。 测试时，可填入自己的电子邮件地址作为收件人。

   单击“添加动态内容”，显示消息中可包含的内容。 选择“内容”- 将包含电子邮件中的消息。 

   ![显示逻辑应用的电子邮件选项的屏幕截图。](./media/tutorial-routing/logic-app-send-email.png)

9. 单击“ **保存**”。 然后关闭逻辑应用设计器。

## <a name="set-up-azure-stream-analytics"></a>设置 Azure 流分析

若要在 Power BI 可视化中查看数据，首先需要设置流分析作业来检索数据。 请记住，只会将“级别”为“常规”的消息发送到默认终结点，并由针对 Power BI 可视化的流分析作业检索。

### <a name="create-the-stream-analytics-job"></a>创建流分析作业

1. 在 [Azure 门户](https://portal.azure.com)中，单击“创建资源” > “物联网” > “流分析作业”。

2. 为作业输入以下信息。

   **作业名称**：作业的名称。 该名称必须全局唯一。 本教程使用 contosoJob。

   **资源组**：使用 IoT 中心所用的同一资源组。 本教程使用 ContosoResources。 

   **位置**：使用设置脚本中所用的同一位置。 本教程使用“美国西部”。 

   ![显示如何创建流分析作业的屏幕截图。](./media/tutorial-routing/stream-analytics-create-job.png)

3. 单击“创建”来创建作业。 若要返回到作业，请单击“资源组”。 本教程使用 ContosoResources。 选择资源组，然后在资源列表中单击流分析作业。 

### <a name="add-an-input-to-the-stream-analytics-job"></a>将输入添加到流分析作业

4. 在“作业拓扑”下，单击“输入”。

5. 在“输入”窗格中，单击“添加流输入”，选择 IoT 中心。 在出现的屏幕上，填写以下字段：

   **输入别名**：本教程使用 contosoinputs。

   **订阅**：选择订阅。

   **IoT 中心**：选择 IoT 中心。 本教程使用 ContosoTestHub。

   **终结点**：选择“消息传送”。 （如果选择操作监视，将获得有关 IoT 中心的遥测数据，而不是正在发送的数据。） 

   **共享访问策略名称**：选择“iothubowner”。 门户将填充共享访问策略密钥。

   **使用者组**：选择之前创建的使用者组。 本教程使用 contosoconsumers。
   
   其余字段接受默认值。 

   ![显示如何为流分析作业设置输入的屏幕截图。](./media/tutorial-routing/stream-analytics-job-inputs.png)

6. 单击“ **保存**”。

### <a name="add-an-output-to-the-stream-analytics-job"></a>将输出添加到流分析作业

1. 在“作业拓扑”下，单击“输出”。

2. 在“输出”窗格中，单击“添加”并选择“Power BI”。 在出现的屏幕上，填写以下字段：

   **输出别名**：输出的唯一别名。 本教程使用 contosooutputs。 

   **数据集名称**：要在 Power BI 中使用的数据集的名称。 本教程使用 contosodataset。 

   **表名称**：要在 Power BI 中使用的表的名称。 本教程使用 contosotable。

   在剩余字段中使用默认值。

3. 单击“授权”，并登录到你的 Power BI 帐户。

   ![显示如何为流分析作业设置输出的屏幕截图。](./media/tutorial-routing/stream-analytics-job-outputs.png)

4. 单击“ **保存**”。

### <a name="configure-the-query-of-the-stream-analytics-job"></a>配置流分析作业的查询

1. 在“作业拓扑”下，单击“查询”。

2. 将 `[YourInputAlias]` 替换为作业的输入别名。 本教程使用 contosoinputs。

3. 将 `[YourOutputAlias]` 替换为作业的输出别名。 本教程使用 contosooutputs。

   ![显示如何为流分析作业设置查询的屏幕截图。](./media/tutorial-routing/stream-analytics-job-query.png)

4. 单击“ **保存**”。

5. 关闭“查询”窗格。 这将返回到“资源组”中的资源的视图。 单击流分析作业。 本教程中将其称为 **contosoJob**。

### <a name="run-the-stream-analytics-job"></a>运行流分析作业

在流分析作业中，单击“启动” > “现在” > “启动”。 成功启动作业后，作业状态将从“已停止”更改为“正在运行”。

设置 Power BI 报表需要数据，因此将先创建设备并运行设备模拟应用程序再设置 Power BI。

## <a name="run-simulated-device-app"></a>运行模拟设备应用

之前的脚本设置部分中，已设置了一个使用 IoT 设备进行模拟的设备。 本部分将下载一个 .NET 控制台应用，用于模拟向 IoT 中心发送设备到云消息的设备。 该应用程序为每个不同的路由方法发送消息。 

下载 [IoT 设备模拟](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)的解决方案。 这将下载一个其中含有数个应用程序的存储库；你要查找的解决方案位于 iot-hub/Tutorials/Routing/SimulatedDevice/ 中。

双击解决方案文件 (SimulatedDevice.sln)，在 Visual Studio 中打开代码，然后打开 Program.cs。 使用 IoT 中心主机名代替 `{iot hub hostname}`。 IoT 中心主机名的格式为“{iot-hub-name}.azure-devices.net”。 本教程的中心主机名为“ContosoTestHub.azure-devices.net”。 接下来，使用之前设置模拟设备时保存的设备密钥代替 `{device key}`。 

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHub.azure-devices.net";
        // This is the primary key for the device. This is in the portal. 
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key. 
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>运行和测试 

运行控制台应用程序。 稍等几分钟。 可在应用程序的控制台屏幕上看到正在发送的消息。

此应用每隔一秒发送一条新的设备到云消息到 IoT 中心。 此消息包含一个 JSON 序列化对象，其中具有设备 ID、温度、湿度和消息级别（级别默认为 `normal`）。 它随机分配 `critical` 级别或 `storage` 级别，使消息路由到存储帐户或路由到服务总线队列（这会触发逻辑应用发送电子邮件）。 默认 (`normal`) 读取内容将在之后设置的 BI 报表中显示。

如果所有内容都正确设置，则此时应可以看见以下结果：

1. 开始收到关键消息相关的电子邮件。 

   ![显示结果邮件的屏幕截图。](./media/tutorial-routing/results-in-email.png)

   这表示：

   * 到服务总线队列的路由运行正常。
   * 从服务总线队列检索消息的逻辑应用运行正常。
   * 连接到 Outlook 的逻辑应用连接器工作正常。 

2. 在 [Azure 门户](https://portal.azure.com)中，单击“资源组”，并选择你的资源组。 本教程使用 ContosoResources。 选择存储帐户，单击“Blob”，然后选择容器。 本教程使用 contosoresults。 现在应该可以看见一个文件夹，可继续深入查看目录，直到看见一个或多个文件。 打开其中某个文件；这些文件中包含路由到存储帐户的条目。 

   ![显示存储中的结果文件的屏幕截图。](./media/tutorial-routing/results-in-storage.png)

这表示：

   * 到存储帐户的路由运行正常。

现在，在应用程序仍在运行的情况下，设置 Power BI 可视化以显示来自默认路由的消息。 

## <a name="set-up-the-power-bi-visualizations"></a>设置 Power BI 可视化

1. 登录到 [Power BI](https://powerbi.microsoft.com/) 帐户。

2. 转到“工作区”，然后选择为流分析作业创建输出时设置的工作区。 本教程使用“我的工作区”。 

3. 单击“数据集”。

   此时会看到列出的数据集，该数据集是在为流分析作业创建输出时指定的。 本教程使用 contosodataset。 （数据集首次显示的过程可能需要 5-10 分钟。）

4. 在“操作”下，单击第一个用于创建报表的图标。

   ![此屏幕截图显示了 PowerBI 工作区，其中突出显示了“操作”和报表图标。](./media/tutorial-routing/power-bi-actions.png)

5. 创建折线图，显示某段时间的实时温度。

   * 在报表创建页上，单击“折线图”图标添加折线图。

   ![显示可视化效果和字段的屏幕截图。](./media/tutorial-routing/power-bi-visualizations-and-fields.png)

   * 在“字段”窗格中展开一个表，该表是在为流分析作业创建输出时指定的。 本教程使用 contosotable。

   * 将 **EventEnqueuedUtcTime** 拖至“可视化效果”窗格中的“轴”。

   * 将“温度”拖至“值”。

   已创建一个折线图。 X 轴显示 UTC 时区的日期和时间。 Y 轴显示来自传感器的温度。

6. 创建另一个折线图，显示某段时间的实时湿度。 要设置第二个折线图，请执行上诉相同步骤，将“EventEnqueuedUtcTime”置于 x 轴，将“湿度”置于 y 轴。

   ![此屏幕截图显示了其中有两个折线图的最终 Power BI 报表。](./media/tutorial-routing/power-bi-report.png)

7. 单击“保存”以保存该报表。

现在应在两个图表上都能看到数据。 这表示：

   * 到默认终结点的路由运行正常。
   * Azure 流分析作业的流式传输正常。
   * Power BI 可视化设置正确。

单击 Power BI 窗口顶部的“刷新”按钮刷新图表，可查看最近的数据。 

## <a name="clean-up-resources"></a>清理资源 

如果想要删除已创建的所有资源，请删除资源组。 此操作会一并删除组中包含的所有资源。 在这种情况下，它会删除 IoT 中心、服务总线命名空间和队列、逻辑应用、存储帐户和资源组本身。 

### <a name="clean-up-resources-in-the-power-bi-visualization"></a>清理 Power BI 可视化效果中的资源

登录到 [Power BI](https://powerbi.microsoft.com/) 帐户。 转到你的工作区。 本教程使用“我的工作区”。 若要删除 Power BI 可视化，请转到数据集并单击“垃圾桶”图标删除该数据集。 本教程使用 contosodataset。 删除数据集时，报表也随之删除。

### <a name="clean-up-resources-using-azure-cli"></a>使用 Azure CLI 清理资源

若要删除资源组，请使用 [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) 命令。

```azurecli-interactive
az group delete --name $resourceGroup
```
### <a name="clean-up-resources-using-powershell"></a>使用 PowerShell 清理资源

若要删除资源组，使用 [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/remove-azurermresourcegroup) 命令。 $resourceGroup 在本教程开始时就已设置为“ContosoIoTRG1”。

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name $resourceGroup
```

## <a name="next-steps"></a>后续步骤

通过本教程，了解了如何通过执行以下任务，使用消息路由将 IoT 中心消息路由到不同目标。  

> [!div class="checklist"]
> * 使用 Azure CLI 或 PowerShell 设置基础资源 - IoT 中心、存储帐户、服务总线队列和模拟设备。
> * 在 IoT 中心为存储帐户和服务总线队列配置终结点和路由。
> * 创建一个逻辑应用，该应用将在消息添加到服务总线队列时触发，并发送电子邮件。
> * 下载并运行应用，该应用模拟 IoT 设备将消息发送到中心，以获得不同的路由选择。
> * 为发送至默认终结点的数据创建 Power BI 可视化。
> * 查看结果...
> * ...在服务总线队列和电子邮件中。
> * ...在存储帐户中。
> * ...在 Power BI 可视化中。

转到下一教程，了解如何管理 IoT 设备的状态。 

> [!div class="nextstepaction"]
[通过 IoT 中心设置和使用指标和诊断](tutorial-use-metrics-and-diags.md)
