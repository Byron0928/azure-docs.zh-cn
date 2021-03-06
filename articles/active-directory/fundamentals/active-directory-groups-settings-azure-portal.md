---
title: 如何使用 Azure Active Directory 编辑组信息 | Microsoft Docs
description: 了解如何使用 Azure Active Directory 编辑组信息。
services: active-directory
author: eross-msft
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: conceptual
ms.date: 08/27/2018
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: a02987fdce3a15cd5d416234e3717df6d33622ec
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2018
ms.locfileid: "45731333"
---
# <a name="how-to-edit-your-group-information-using-azure-active-directory"></a>如何：使用 Azure Active Directory 编辑组信息

使用 Azure Active Directory，可编辑组的设置，包括更新其名称、描述或成员类型。

## <a name="to-edit-your-group-settings"></a>编辑组设置
1. 使用目录的全局管理员帐户登录到 [Azure 门户](https://portal.azure.com)。

2. 选择“Azure Active Directory”，然后选择“组”。

    此时会出现“组 - 所有组”页，其中显示了所有活动的组。

3. 在“组 - 所有组”页上，向“搜索”框中键入尽可能多的名称。 出于本文的目的，我们正搜索“MDM 策略 - 西部”组。

    搜索结果将显示在“搜索”框下，键入更多字符时搜索结果将更新。

    ![所有组页面，搜索框中都有搜索文本](media/active-directory-groups-settings-azure-portal/search-for-specific-group.png)

4. 选择“MDM 策略 - 西部”，然后选择在“管理”区域选择“属性”。

    ![组概述页面，其中突出显示数字和成员以及成员选项](media/active-directory-groups-settings-azure-portal/group-overview-blade.png)

5. 按所需更新“常规设置”信息，包括：

    ![组的属性设置](media/active-directory-groups-settings-azure-portal/group-properties-settings.png)

    - **组名。** 编辑现有组名称。
    
    - **组描述。** 编辑现有组说明。

    - **组类型。** 创建组类型后将无法更改。 若要更改“组类型”，则必须删除该组并创建新组。
    
    - **成员身份类型。** 更改成员身份类型。 有关各种可用成员身份类型，请参阅[如何：使用 Azure Active Directory 门户创建基本组并添加成员](active-directory-groups-create-azure-portal.md)
    
    - **对象 ID。** 无法更改对象 ID，但可将其复制以在组的 PowerShell 命令中使用。 有关 PowerShell cmdlet 的详细信息，请参阅[用于配置组设置的 Azure Active Directory cmdlet](../users-groups-roles/groups-settings-v2-cmdlets.md)。

## <a name="next-steps"></a>后续步骤
这些文章提供了有关 Azure Active Directory 的更多信息。

- [查看组和成员](active-directory-groups-view-azure-portal.md)

- [创建基本组并添加成员](active-directory-groups-create-azure-portal.md)

- [如何在组中添加或移除成员](active-directory-groups-members-azure-portal.md)

- [管理组中用户的动态规则](../users-groups-roles/groups-create-rule.md)

- [管理组的成员身份](active-directory-groups-membership-azure-portal.md)

- [使用组管理对资源的访问权限](active-directory-manage-groups.md)

- [将 Azure 订阅关联或添加到 Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)
