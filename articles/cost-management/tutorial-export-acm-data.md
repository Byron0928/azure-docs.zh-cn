---
title: 教程 - 从 Azure 成本管理创建和管理导出的数据 | Microsoft 文档
description: 本文介绍如何创建和管理导出的 Azure 成本管理数据，以便在外部系统中使用。
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 7f93a225db845840545b761d812f5a8a81f76f91
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913557"
---
# <a name="tutorial-create-and-manage-exported-data"></a>教程：创建和管理导出的数据

如果你已阅读成本分析教程，则会熟悉如何手动下载成本管理数据。 但是，你也可以创建一个每日定期任务，每天自动将成本管理数据导出到 Azure 存储。 导出的数据采用 CSV 格式，它包含成本管理收集的所有信息。 然后，可以在外部系统使用从 Azure 存储导出的数据，并将其与自己的自定义数据相结合。 在类似仪表板或其他财务系统等的外部系统中，你都可以使用导出的数据。

本教程将通过示例，指导你导出成本管理数据，然后验证数据是否已成功导出。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建每日导出
> * 验证收集的数据

## <a name="prerequisites"></a>先决条件

所有[企业协议 (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) 客户均可使用数据导出。 每个订阅支持以下 Azure 权限，以便用户和组导出数据：

- 所有者 - 可以为订阅创建、修改或删除计划导出。
- 参与者 - 可以创建、修改或删除自己的计划导出。 可以修改其他人创建的计划导出的名称。
- 读者 - 可以计划他们有权访问的导出。

对于 Azure 存储帐户：
- 无论导出权限如何，更改配置的存储帐户都需要写入权限。
- 必须为 Blob 或文件存储配置 Azure 存储帐户。

## <a name="sign-in-to-azure"></a>登录 Azure
在 [https://portal.azure.com](https://portal.azure.com/) 中登录 Azure 门户。

## <a name="create-a-daily-export"></a>创建每日导出

“成本管理 + 计费”&gt;“成本管理”&gt; 在订阅中选择订阅或资源组 &gt;“导出”&gt;“添加”。

键入导出名称并指定订阅、Azure 存储帐户、容器和文件存储目录或 blob 容器，然后单击“创建”。

![新导出](./media/tutorial-export-acm-data/new-export01.png)

新导出将出现在导出列表中。 默认情况下，新导出为启用状态，并且每天运行一次。 如果要禁用或删除计划的导出，请单击列表中的任何项，然后再单击“禁用”或“删除”。

最初，在导出运行之前，可能需要一到两个小时。 但是，在导出文件中显示数据之前，它可能需要四个小时。

## <a name="verify-that-data-is-collected"></a>验证收集的数据

可以轻松验证正在收集的成本管理数据，并使用 Azure 存储资源管理器查看导出的 CSV 文件。

在导出列表中，单击存储帐户名称。 在存储帐户页上，单击“在资源管理器中打开”。 如果看到确认框，单击“是”，即可在 Azure 存储资源管理器中打开文件。

![存储帐户页](./media/tutorial-export-acm-data/storage-account-page.png)

在存储资源管理器中，导航到你想要打开的容器，并选择与当前月份相对应的文件夹。 然后将显示 CSV 文件列表。 选择一个文件，然后单击“打开”。

![存储资源管理器](./media/tutorial-export-acm-data/storage-explorer.png)

该文件将通过设置为打开 CSV 文件扩展名的程序或应用程序打开。 下面是 Excel 中的一个示例。

![示例导出数据](./media/tutorial-export-acm-data/example-export-data.png)

## <a name="access-exported-data-from-other-systems"></a>从其他系统访问导出的数据

导出成本管理数据的用途之一是从外部系统访问数据。 你可能使用的是仪表板系统或其他财务系统。 此类系统各不相同，因此，只通过一个示例说明并不能解决实际问题。  但是，你可以在 [Azure 存储简介](../storage/common/storage-introduction.md)中开始学习如何访问应用程序中的数据。

## <a name="next-steps"></a>后续步骤

本教程介绍了如何：

> [!div class="checklist"]
> * 创建每日导出
> * 验证收集的数据

进入下一个教程，通过识别闲置和未充分利用的资源来优化和提高效率。

> [!div class="nextstepaction"]
> [查看并采纳优化建议](tutorial-acm-opt-recommendations.md)
