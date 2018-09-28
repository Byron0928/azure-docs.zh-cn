---
title: 使用 InfluxData Telegraf 代理收集 Linux VM 的自定义指标
description: 使用 InfluxData Telegraf 代理收集 Linux VM 的自定义指标
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 651706b269cf24eca85e0601384ef63cb6ed06a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "46990699"
---
# <a name="collect-custom-metrics-for-a-linux-vm-with-the-influxdata-telegraf-agent"></a>使用 InfluxData Telegraf 代理收集 Linux VM 的自定义指标

有了 Azure Monitor，就可以通过应用程序遥测、在 Azure 资源上运行的代理甚至从外到内的监视系统收集自定义指标，然后将其直接提交给 Azure Monitor。 本文重点说明如何在 Azure 中的 Linux VM 上部署 [InfluxData](https://www.influxdata.com/) Telegraf 代理，以及如何配置代理，以便将指标发布到 Azure Monitor。 

## <a name="influxdata-telegraf-agent"></a>InfluxData Telegraf 代理 

[Telegraf](https://docs.influxdata.com/telegraf/v1.7/) 是一种插件驱动型代理，可以从 150 多个不同的源收集指标。 根据在 VM 上运行的具体工作负荷（例如 MySQL、NGINX、Apache 等），可以将代理配置为利用专用输入插件来收集指标。 输出插件，然后使代理能够向所选目标写入数据。 Telegraf 代理直接集成了 Azure Monitor 自定义指标 REST API，并支持“Azure Monitor 输出插件”。 这样代理就可以在 Linux VM 上收集特定于工作负荷的指标，然后将其作为自定义指标提交到 Azure Monitor。 

 ![Telegraph 代理概述](./media/metrics-store-custom-linux-telegraf/telegraf-agent-overview.png)

## <a name="send-custom-metrics"></a>发送自定义指标 

就本教程来说，我们部署运行 Ubuntu 16.04 LTS 操作系统的 Linux VM。 大多数 Linux 操作系统支持 Telegraf 代理。 InfluxData 下载门户提供 Debian 和 RPM 包以及未打包的 Linux 二进制文件。 有关其他 Telegraf 安装说明和选项，请参阅本安装指南。 

登录到 [Azure 门户](https://portal.azure.com)

若要创建新的 Linux VM，请执行以下操作： 

1. 在左侧导航窗格中单击  **创建资源**   选项。 
1. 搜索*虚拟机*  
1. 选择“Ubuntu 16.04 LTS”，然后单击“创建” 
1. 提供一个 VM 名称，例如  *MyTelegrafVM*。  
1. 将磁盘类型保留为 **SSD**，然后提供**用户名**，例如  *azureuser*。 
1. 选择“密码”作为“身份验证类型”，然后键入一个密码，以后需使用该密码通过 SSH 登录到此 VM。 ** **** 
1. 选择“新建资源组” ****，然后提供一个名称，例如  *myResourceGroup*。  选择所需“位置”，然后选择“确定”。 **** 

     ![创建 Ubuntu VM](./media/metrics-store-custom-linux-telegraf/create-vm.png)

1. 为 VM 选择大小。 例如，可以按“计算类型”或“磁盘类型”进行筛选。 

     ![虚拟机大小 Telegraph 代理概述](./media/metrics-store-custom-linux-telegraf/vm-size.png)

1. 在“设置”页上的“网络”>“网络安全组”>“选择公共入站端口”中，选择“HTTP”和“SSH (22)”。 **** **** **** **** ** **       将剩余的字段保留默认设置，然后选择“确定”。 **** 

1. 在摘要页上，选择“创建”以启动 VM 部署。 

1. VM 将固定到 Azure 门户仪表板。 完成部署后，会自动打开 VM 摘要。 

1. 在“VM”边栏选项卡中导航到“标识”选项卡，确保 VM 的系统分配标识处于“启用”状态。 
 
![fillin](./media/metrics-store-custom-linux-telegraf/connect-to-VM.png)
 
## <a name="connect-to-the-vm"></a>连接到 VM 

创建与 VM 的 SSH 连接。 选择 VM 的概览页面上的“连接”按钮。 

![fillin](./media/metrics-store-custom-linux-telegraf/connect-VM-button2.png)

在“连接到虚拟机”页面中，保留默认选项，以使用 DNS 名称通过端口 22 进行连接。 在“使用 VM 本地帐户登录”中，会显示一个连接命令。 单击相应按钮来复制该命令。 下面的示例展示了 SSH 连接命令的样式： 

```cmd
ssh azureuser@XXXX.XX.XXX 
```

将 SSH 连接命令粘贴到 Windows 上的某个 Shell（例如 Azure Cloud Shell 或 Ubuntu 上的 Bash）中来创建连接，或者使用所选 SSH 客户端来创建连接。 

## <a name="install-and-configure-telegraf"></a>安装和配置 Telegraf 

若要将 Telegraf Debian 包安装到 VM 上，请从 SSH 会话运行以下命令： 

```cmd
# download the package to the VM 
wget https://dl.influxdata.com/telegraf/releases/telegraf_1.8.0~rc1-1_amd64.deb 
# install the package 
sudo dpkg -i telegraf_1.8.0~rc1-1_amd64.deb
```
Telegraf 的配置文件定义 Telegraf 的操作。 默认情况下，示例配置文件安装在“/etc/telegraf/telegraf.conf”路径。 示例配置文件列出了所有可能的输入和输出插件。 但是，我们会运行以下命令，以便创建自定义配置文件并让代理来使用它。 

```cmd
# generate the new Telegraf config file in the current directory 
telegraf --input-filter cpu:mem --output-filter azure_monitor config > azm-telegraf.conf 

# replace the example config with the new generated config 
sudo cp azm-telegraf.conf /etc/telegraf/telegraf.conf 
```

> [!NOTE]
> 上面的命令仅启用两个输入插件，即“cpu”和“mem”。请根据计算机上运行的工作负荷随意添加更多输入插件（Docker、MySQL、NGINX 等）。 输入插件的完整列表可以在这里找到。 

最后，为了让代理开始使用新配置，我们将运行以下命令，强制代理在停止后再启动 

```cmd
# stop the telegraf agent on the VM 
sudo systemctl stop telegraf 
# start the telegraf agent on the VM to ensure it picks up the latest configuration 
sudo systemctl start telegraf 
```
现在，代理会从每个指定的输入插件收集指标，然后将指标发出到 Azure Monitor。 

## <a name="plot-your-telegraf-metrics-in-the-azure-portal"></a>在 Azure 门户中绘制 Telegraf 指标 

1. 打开 [Azure 门户](https://portal.azure.com) 

1. 导航到新的“监视”选项卡，然后选择“指标”。 ****  
     ![fillin](./media/metrics-store-custom-linux-telegraf/metrics.png)

1. 在资源选择器中选择 VM

     ![fillin](./media/metrics-store-custom-linux-telegraf/metric-chart.png)

1. 选择 *Telegraf/CPU* 命名空间，然后选择 *usage_system* 指标。 可以按此指标上的维度选择筛选器，也可以对维度进行拆分。  

     ![fillin](./media/metrics-store-custom-linux-telegraf/VM-resource-selector.png)

## <a name="additional-configuration"></a>其他配置 

以上演练介绍了如何配置 Telegraf 代理，以便从一些基本的输入插件收集指标。 Telegraf 代理支持 150 多个输入插件，部分插件支持其他配置选项。 InfluxData 发布了一个[受支持插件的列表](https://docs.influxdata.com/telegraf/v1.7/plugins/inputs/)以及有关[如何配置它们](https://docs.influxdata.com/telegraf/v1.7/administration/configuration/)的说明。  

另外，本演练还让你使用 Telegraf 代理发出在其上部署了代理的 VM 的相关指标。 Telegraf 代理也可用作其他资源的指标的收集器和转发器。 若要了解如何将代理配置为发出其他 Azure 资源的指标，请参阅 [Azure Monitor Custom Metric Output for Telegraf](https://github.com/influxdata/telegraf/blob/fb704500386214655e2adb53b6eb6b15f7a6c694/plugins/outputs/azure_monitor/README.md)（针对 Telegraf 的 Azure Monitor 自定义指标输出）。  

## <a name="clean-up-resources"></a>清理资源 

不再需要资源组、虚拟机和所有相关的资源时，可将其删除。 为此，请选择虚拟机的资源组，选择“删除”，然后确认要删除的资源组的名称。 

## <a name="next-steps"></a>后续步骤
- 详细了解[自定义指标](metrics-custom-overview.md)。

