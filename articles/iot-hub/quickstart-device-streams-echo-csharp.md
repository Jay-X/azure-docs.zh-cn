---
title: Azure IoT 中心设备流 C# 快速入门（预览）| Microsoft Docs
description: 在本快速入门中，你将运行两个示例 C# 应用程序，以便使用通过 IoT 中心建立的设备流进行通信。
author: rezasherafat
manager: briz
ms.service: iot-hub
services: iot-hub
ms.devlang: csharp
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/15/2019
ms.author: rezas
ms.openlocfilehash: edd3912b3674f3a80a81fd47ed490479f663852c
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/23/2019
ms.locfileid: "54830820"
---
# <a name="quickstart-communicate-to-device-applications-in-c-via-iot-hub-device-streams-preview"></a>快速入门：通过 IoT 中心设备流在 C# 中与设备应用程序通信（预览）

[!INCLUDE [iot-hub-quickstarts-3-selector](../../includes/iot-hub-quickstarts-3-selector.md)]

服务和设备应用程序可以使用 [IoT 中心设备流](./iot-hub-device-streams-overview.md)以安全且防火墙友好的方式进行通信。 本快速入门涉及两个 C# 程序，这两个程序可以利用设备流来回发送数据（回显）。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

如果还没有 Azure 订阅，可以在开始前创建一个[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="prerequisites"></a>先决条件

本快速入门中运行的两个示例应用程序是使用 C# 编写的。 开发计算机上需要有 .NET Core SDK 2.1.0 或更高版本。

可以从 [.NET](https://www.microsoft.com/net/download/all) 为多个平台下载 .NET Core SDK。

可以使用以下命令验证开发计算机上 C# 的当前版本：

```
dotnet --version
```

从 https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip 下载示例 C# 项目并提取 ZIP 存档。


## <a name="create-an-iot-hub"></a>创建 IoT 中心

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub-device-streams.md)]

## <a name="register-a-device"></a>注册设备

必须先将设备注册到 IoT 中心，然后该设备才能进行连接。 在本快速入门中，将使用 Azure Cloud Shell 来注册模拟设备。

1. 在 Azure Cloud Shell 中运行以下命令，以添加 IoT 中心 CLI 扩展并创建设备标识。 

   **YourIoTHubName**：将下面的占位符替换为你为 IoT 中心选择的名称。

   **MyDevice**：这是为注册的设备提供的名称。 如示例中所示使用 MyDevice。 如果为设备选择不同名称，则可能还需要在本文中从头至尾使用该名称，并在运行示例应用程序之前在其中更新设备名称。

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    az iot hub device-identity create --hub-name YourIoTHubName --device-id MyDevice
    ```

2. 在 Azure Cloud Shell 中运行以下命令，以获取刚注册设备的_设备连接字符串_：

   **YourIoTHubName**：将下面的占位符替换为你为 IoT 中心选择的名称。

    ```azurecli-interactive
    az iot hub device-identity show-connection-string --hub-name YourIoTHubName --device-id MyDevice --output table
    ```

    记下设备连接字符串，如以下示例所示：

   `HostName={YourIoTHubName}.azure-devices.net;DeviceId=MyDevice;SharedAccessKey={YourSharedAccessKey}`

    稍后会在快速入门中用到此值。

3. 此外，需要使用 IoT 中心的服务连接字符串才能让服务端应用程序连接到 IoT 中心并建立设备流。 以下命令检索 IoT 中心的此值：

   **YourIoTHubName**：将下面的占位符替换为你为 IoT 中心选择的名称。

    ```azurecli-interactive
    az iot hub show-connection-string --policy-name service --hub-name YourIoTHubName
    ```

    记下返回的值，如下所示：

   `"HostName={YourIoTHubName}.azure-devices.net;SharedAccessKeyName=service;SharedAccessKey={YourSharedAccessKey}"`
    

## <a name="communicate-between-device-and-service-via-device-streams"></a>通过设备流在设备和服务之间通信

### <a name="run-the-service-side-application"></a>运行服务端应用程序

导航到解压缩的项目文件夹中的 `device-streams-echo/service`。 需要准备好以下信息：

| 参数名称 | 参数值 |
|----------------|-----------------|
| `ServiceConnectionString` | IoT 中心的服务连接字符串。 |
| `DeviceId` | 前面创建的设备标识符。 |

按如下所示编译并运行代码：

```
cd ./iot-hub/Quickstarts/device-streams-echo/service/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $ServiceConnectionString MyDevice

# In Windows
dotnet run %ServiceConnectionString% MyDevice
```

### <a name="run-the-device-side-application"></a>运行设备端应用程序

导航到解压缩的项目文件夹中的 `device-streams-echo/device` 目录。 需要准备好以下信息：

| 参数名称 | 参数值 |
|----------------|-----------------|
| `DeviceConnectionString` | 前面创建的设备的连接字符串。 |

按如下所示编译并运行代码：

```
cd ./iot-hub/Quickstarts/device-streams-echo/device/

# Build the application
dotnet build

# Run the application
# In Linux/MacOS
dotnet run $DeviceConnectionString

# In Windows
dotnet run %DeviceConnectionString%
```

在最后一步结束时，服务端程序会启动一个发往设备的流，在该流建立后就会通过其将字符串缓冲区发送到服务。 在此示例中，服务端程序直接将相同的数据回显到设备，表明已成功地在两个应用程序之间建立双向通信。 见下图。

设备端的控制台输出：![alt 文本](./media/quickstart-device-streams-echo-csharp/device-console-output.png "设备端的控制台输出")


服务端的控制台输出：![alt 文本](./media/quickstart-device-streams-echo-csharp/service-console-output.png "服务端的控制台输出")



通过该流发送的流量将通过 IoT 中心以隧道方式进行传输，而不是直接发送。 这就带来了[这些优势](./iot-hub-device-streams-overview.md#benefits)。


## <a name="clean-up-resources"></a>清理资源

[!INCLUDE [iot-hub-quickstarts-clean-up-resources](../../includes/iot-hub-quickstarts-clean-up-resources-device-streams.md)]


## <a name="next-steps"></a>后续步骤

在本快速入门中，你设置了一个 IoT 中心、注册了一个设备、在设备和服务端的 C# 应用程序之间建立了一个设备流，并通过该流在应用程序之间来回发送数据。

请使用以下链接详细了解设备流：

> [!div class="nextstepaction"]
> [设备流概述](./iot-hub-device-streams-overview.md)
