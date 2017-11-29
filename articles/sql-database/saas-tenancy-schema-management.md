---
title: "在多租户应用中管理 Azure SQL 数据库架构 | Microsoft Docs"
description: "在使用 Azure SQL 数据库的多租户应用程序中为多个租户管理架构"
keywords: "sql 数据库教程"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: billgib; sstein
ms.openlocfilehash: 701a7296368cd8150eedf8cc50b989fdf6112101
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2017
---
# <a name="manage-schema-for-multiple-tenants-in-a-multi-tenant-application-that-uses-azure-sql-database"></a>在使用 Azure SQL 数据库的多租户应用程序中为多个租户管理架构

[第一个 Wingtip Tickets SaaS Database Per Tenant 教程](saas-dbpertenant-get-started-deploy.md)介绍该应用如何预配租户数据库并将其注册到目录中。 与任何应用程序一样，Wingtip Tickets SaaS Database Per Tenant 应用也会不断改进，并且会不时需要对数据库进行更改。 更改可能包括新的或更改的架构、新的或更改的引用数据，以及常规数据库维护任务，目的是确保应用的最佳性能。 使用 SaaS 应用程序时，需要跨数量可能很多的租户数据库以协调的方式部署这些更改。 要使这些更改出现在未来租户数据库中，需要将这些更改合并到预配过程中。

本教程探讨两个方案 - 为所有租户部署引用数据更新，以及返回包含引用数据的表的索引。 [弹性作业](sql-database-elastic-jobs-overview.md)功能用于跨所有租户执行这些操作，golden 租户数据库用作新数据库的模板。

本教程介绍如何执行下列操作：

> [!div class="checklist"]

> * 创建作业帐户
> * 跨多个租户查询
> * 更新所有租户数据库中的数据
> * 在所有租户数据库中的表上创建索引


若要完成本教程，请确保满足以下先决条件：

* 已部署 Wingtip Tickets SaaS Database Per Tenant 应用。 若要在五分钟内完成部署，请参阅[部署和浏览 Wingtip Tickets SaaS Database Per Tenant 应用程序](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell 已安装。 有关详细信息，请参阅 [Azure PowerShell 入门](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* 已安装最新版的 SQL Server Management Studio (SSMS)。 [下载并安装 SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

*本教程使用 SQL 数据库服务的功能，这些功能为有限预览版（弹性数据库作业）。如果希望完成本教程，请将订阅 ID 提供 给 SaaSFeedback@microsoft.com，并在邮件主题中注明“弹性作业预览版”。收到订阅已启用的确认邮件后，即可[下载并安装最新的预发行作业 cmdlet](https://github.com/jaredmoo/azure-powershell/releases)。此预览版的功能有限，因此，如果存在相关问题或者需要支持，请联系 SaaSFeedback@microsoft.com。*


## <a name="introduction-to-saas-schema-management-patterns"></a>SaaS 架构管理模式简介

每个数据库一个租户的 SaaS 模式在许多方面受益于由此而导致的数据隔离，但同时也导致复杂性增强，因为必须维护和管理多个数据库。 [弹性作业](sql-database-elastic-jobs-overview.md)有利于 SQL 数据层的管理。 可以通过作业安全且可靠地针对一组数据库运行任务（T-SQL 脚本），不需考虑用户交互或输入。 可以使用此方法跨应用程序中的所有租户部署架构和常见的引用数据更改。 此外还可以通过弹性作业来维护数据库用来创建新租户的 golden 副本，确保其架构和引用数据始终为最新。

![屏幕](media/saas-tenancy-schema-management/schema-management.png)


## <a name="elastic-jobs-limited-preview"></a>弹性作业有限预览版

有一个新版的弹性作业，该作业现为 Azure SQL 数据库的集成功能（不需其他服务或组件）。 此新版弹性作业目前为有限预览版。 此有限预览版目前支持 PowerShell 创建作业帐户，同时支持 T-SQL 创建和管理作业。

> [!NOTE]
> *本教程使用 SQL 数据库服务的功能，这些功能为有限预览版（弹性数据库作业）。如果希望完成本教程，请将订阅 ID 提供 给 SaaSFeedback@microsoft.com，并在邮件主题中注明“弹性作业预览版”。收到订阅已启用的确认邮件后，即可[下载并安装最新的预发行作业 cmdlet](https://github.com/jaredmoo/azure-powershell/releases)。此预览版的功能有限，因此，如果存在相关问题或者需要支持，请联系 SaaSFeedback@microsoft.com。*

## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>获取 Wingtip Tickets SaaS Database Per Tenant 应用程序的脚本

可在 [WingtipTicketsSaaS-DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) github 存储库中查看 Wingtip Tickets SaaS Database Per Tenant 的脚本和应用程序源代码。 [下载 Wingtip Tickets SaaS Database Per Tenant 脚本的步骤](saas-dbpertenant-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-tickets-saas-database-per-tenant-scripts)。

## <a name="create-a-job-account-database-and-new-job-account"></a>创建作业帐户数据库和新的作业帐户

本教程要求使用 PowerShell 来创建作业帐户数据库和作业帐户。 与 MSDB 和 SQL 代理一样，弹性作业使用 Azure SQL 数据库来存储作业定义、作业状态和历史记录。 创建作业帐户后，即可立刻创建和监视作业。

1. 在 PowerShell ISE 中打开 …\\Learning Modules\\Schema Management\\Demo-SchemaManagement.ps1。
1. 按 **F5** 运行脚本。

Demo-SchemaManagement.ps1 脚本调用 Deploy-SchemaManagement.ps1 脚本，目的是在编录服务器上创建名为 **jobaccount** 的 *S2* 数据库。 然后，该脚本会创建作业帐户，将 jobaccount 数据库作为参数传递给作业帐户创建调用。

## <a name="create-a-job-to-deploy-new-reference-data-to-all-tenants"></a>创建一个将新的引用数据部署到所有租户的作业

每个租户数据库包括一组地点类型，用于定义托管在某个地点的事件种类。 在本练习中，将一个更新部署到所有租户数据库，以便添加两种额外的地点类型：“赛车”和“游泳俱乐部”。 这些地点类型对应于在租户事件应用中看到的背景图。

单击“地点类型”下拉菜单，验证是否只有 10 个地点类型选项可用，以及具体说来，“赛车”和“游泳俱乐部”是否未包括在列表中。

现在请创建一个作业，对所有租户数据库中的“VenueTypes”表进行更新，并添加新地点类型。

若要创建新的 作业，请使用一组作业系统存储过程，这些过程是在创建作业帐户时，在 jobaccount 数据库中创建的。

1. 打开 SSMS 并连接到编录服务器：catalog-dpt-\<用户\>.database.windows.net 服务器
1. 另请连接到租户服务器：tenants1-dpt-\<用户\>.database.windows.net
1. 浏览到 tenants1-dpt-\<user\> 服务器上的 contosoconcerthall 数据库，查询 VenueTypes 表以确认“赛车”和“游泳俱乐部”不在结果列表中。
1. 在 SSMS 中，打开 …\\Learning Modules\\Schema Management\\DeployReferenceData.sql 文件
1. 修改语句：SET @wtpUser = &lt;user&gt;，并将其替换为部署 Wingtip Tickets SaaS Database Per Tenant 应用时所用的“用户”值
1. 确保已连接到 jobaccount 数据库，并按 **F5** 运行该脚本

* **sp\_add\_target\_group** 创建目标组名称 DemoServerGroup，现在需添加目标成员。
* sp\_add\_target\_group\_member 首先添加 server 目标成员类型，该类型认定在执行作业时该服务器（注意，这是包含租户数据库的 tenants1-dpt-&lt;User&gt; 服务器）中的所有数据库都应包括在作业中；其次是添加 database 目标成员类型，具体说来就是“golden”数据库 (basetenantdb)，后者位于 catalog-dpt-&lt;User&gt; 服务器上；最后是添加另一 database 目标组成员类型，用于包括在后面的教程中使用的 adhocanalytics 数据库中。
* **sp\_add\_job** 创建名为“引用数据部署”的作业
* **sp\_add\_jobstep** 创建包含 T-SQL 命令文本的作业步骤，该文本用于更新引用表 VenueTypes
* 脚本中的剩余视图显示存在的对象以及监视作业执行情况。 使用这些查询可查看 **lifecycle** 列中的状态值，以确定何时作业成功地在所有租户数据库以及两个包含引用表的其他数据库上完成。

1. 在 SSMS 中，浏览到 tenants1-dpt-\<user\> 服务器上的 contosoconcerthall 数据库，查询 VenueTypes 表以确认“赛车”和“游泳俱乐部”此时在结果列表中。


## <a name="create-a-job-to-manage-the-reference-table-index"></a>创建管理引用表索引的作业

与前一练习类似，本练习创建一个可在引用表主键上重新生成索引的作业，这是在将大的数据负载引入表中以后，管理员会执行的典型的数据库管理操作。

使用相同的作业“系统”存储过程创建作业。

1. 打开 SSMS 并连接到 catalog-dpt-&lt;User&gt;.database.windows.net 服务器
1. 打开 …\\Learning Modules\\Schema Management\\OnlineReindex.sql 文件
1. 通过右键单击选择“连接”，并连接到 catalog-dpt-&lt;User&gt;.database.windows.net 服务器（如果尚未连接）
1. 确保已连接到 jobaccount 数据库，并按 F5 运行该脚本

* sp\_add\_job 创建名为“Online Reindex PK\_\_VenueTyp\_\_265E44FD7FD4C885”的新作业
* sp\_add\_jobstep 创建包含 T-SQL 命令文本的作业步骤，该文本用于更新索引




## <a name="next-steps"></a>后续步骤

本教程介绍了如何：

> [!div class="checklist"]

> * 创建可以跨多个租户进行查询的作业帐户
> * 更新所有租户数据库中的数据
> * 在所有租户数据库中的表上创建索引

[“即席分析”教程](saas-tenancy-adhoc-analytics.md)


## <a name="additional-resources"></a>其他资源

* [构建 Wingtip Tickets SaaS Database Per Tenant 应用程序部署的其他教程](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [管理扩大的云数据库](sql-database-elastic-jobs-overview.md)
* [创建和管理扩大的云数据库](sql-database-elastic-jobs-create-and-manage.md)