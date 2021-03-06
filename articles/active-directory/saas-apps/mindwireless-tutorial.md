---
title: 教程：Azure Active Directory 与 mindWireless 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 mindWireless 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: bd00a339-27c9-4904-b66f-a95bf597ac3c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2018
ms.author: jeedes
ms.openlocfilehash: 6c6fe0a720795c67a7062f5a5971c699472fca07
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2018
ms.locfileid: "39434340"
---
# <a name="tutorial-azure-active-directory-integration-with-mindwireless"></a>教程：Azure Active Directory 与 mindWireless 的集成

在本教程中，了解如何将 mindWireless 与 Azure Active Directory (Azure AD) 集成。

将 mindWireless 与 Azure AD 集成提供以下优势：

- 可在 Azure AD 中控制谁有权访问 mindWireless。
- 可以让用户使用其 Azure AD 帐户自动登录到 mindWireless（单一登录）。
- 可在中心位置（即 Azure 门户）管理帐户。

如需了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](../manage-apps/what-is-single-sign-on.md)。

## <a name="prerequisites"></a>先决条件

若要配置 Azure AD 与 mindWireless 的集成，需要具有以下项：

- Azure AD 订阅
- 启用了 mindWireless 单一登录的订阅

> [!NOTE]
> 为了测试本教程中的步骤，我们不建议使用生产环境。

测试本教程中的步骤应遵循以下建议：

- 除非必要，请勿使用生产环境。
- 如果没有 Azure AD 试用环境，可以[获取一个月的试用版](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="scenario-description"></a>方案描述
在本教程中，将在测试环境中测试 Azure AD 单一登录。 本教程中概述的方案包括两个主要构建基块：

1. 从库中添加 mindWireless
1. 配置和测试 Azure AD 单一登录

## <a name="adding-mindwireless-from-the-gallery"></a>从库中添加 mindWireless
要配置 mindWireless 与 Azure AD 的集成，需要从库中将 mindWireless 添加到托管 SaaS 应用列表。

**若要从库添加 mindWireless，请执行以下步骤：**

1. 在 **[Azure 门户](https://portal.azure.com)** 的左侧导航面板中，单击“Azure Active Directory”图标。 

    ![“Azure Active Directory”按钮][1]

1. 导航到“企业应用程序”。 然后转到“所有应用程序”。

    ![“企业应用程序”边栏选项卡][2]
    
1. 若要添加新应用程序，请单击对话框顶部的“新建应用程序”按钮。

    ![“新增应用程序”按钮][3]

1. 在搜索框中，键入“mindWireless”，在结果面板中选择“mindWireless”，然后单击“添加”按钮，添加应用程序。

    ![结果列表中的 mindWireless](./media/mindwireless-tutorial/tutorial_mindwireless_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>配置和测试 Azure AD 单一登录

在本部分中，基于一个名为“Britta Simon”的测试用户使用 mindWireless 配置和测试 Azure AD 单一登录。

若要运行单一登录，Azure AD 需要知道与 Azure AD 用户相对应的 mindWireless 用户。 换句话说，需要建立 Azure AD 用户与 mindWireless 中相关用户之间的链接关系。

若要配置并测试 mindWireless 的 Azure AD 单一登录，需要完成以下构建基块：

1. **[配置 Azure AD 单一登录](#configure-azure-ad-single-sign-on)** - 使用户能够使用此功能。
1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 Britta Simon 测试 Azure AD 单一登录。
1. **[创建 mindWireless 测试用户](#create-a-mindwireless-test-user)** - 在 mindWireless 中创建 Britta Simon 的对应用户，将其链接到用户的 Azure AD 表示形式。
1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 Britta Simon 能够使用 Azure AD 单一登录。
1. **[测试单一登录](#test-single-sign-on)** - 验证配置是否正常工作。

### <a name="configure-azure-ad-single-sign-on"></a>配置 Azure AD 单一登录

在本部分中，将在 Azure 门户中启用 Azure AD 单一登录并在 mindWireless 应用程序中配置单一登录。

**若要配置 mindWireless 的 Azure AD 单一登录，请执行以下步骤：**

1. 在 Azure 门户中的 mindWireless 应用程序集成页上，单击“单一登录”。

    ![配置单一登录链接][4]

1. 在“单一登录”对话框中，选择“基于 SAML 的单一登录”作为“模式”以启用单一登录。

    ![“单一登录”对话框](./media/mindwireless-tutorial/tutorial_mindwireless_samlbase.png)

1. 在“mindWireless 域和 URL”部分中，执行以下步骤：

    ![mindWireless 域和 URL 单一登录信息](./media/mindwireless-tutorial/tutorial_mindwireless_url.png)

    a. 在“标识符”文本框中，使用以下模式键入 URL：`https://<subdomain>.mwsmart.com/`

    b. 在 **“回复 URL”** 文本框中，使用以下模式键入 URL：`https://<subdomain>.mwsmart.com/SAML/AssertionConsumerService.aspx`

    > [!NOTE] 
    > 这些不是实际值。 请使用实际标识符和回复 URL 更新这些值。 请联系 [mindWireless 支持团队](mailto:sdulloor@mindwireless.com)获取这些值。

1. mindWireless 应用程序需要特定格式的 SAML 断言，这要求向 SAML 令牌属性配置添加自定义属性映射。

1. 以下屏幕截图显示了一个示例。 声明名称将始终为“用户 ID”和映射到 user.employeeid 的值，后者包含用户的 EmployeeID。 在此将用户从 Azure AD 映射到 mindWireless 是在 EmployeeID 上完成的，但可将其映射到同样基于应用程序设置的其他值。 首先可与 [mindWireless 支持团队](mailto:sdulloor@mindwireless.com)合作，使用用户的正确标识符，并将该值与“员工 ID”声明一起映射。

    ![配置单一登录](./media/mindwireless-tutorial/tutorial_attribute.png)

1. 在“单一登录”对话框的“用户属性”部分中，按上图所示配置 SAML 令牌属性，再执行以下步骤：
    
    | 属性名称 | 属性值 | 命名空间值 |
    | -------------- | --------------- | ----------------|
    | 员工 ID | user.employeeid | `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    
    a. 单击“添加属性”，打开“添加属性”对话框。

    ![配置单一登录](./media/mindwireless-tutorial/tutorial_attribute_04.png)

    ![配置单一登录](./media/mindwireless-tutorial/tutorial_attribute_05.png)

    b. 在“名称”文本框中，键入为该行显示的属性名称。

    c. 在“值”列表中，选择为该行显示的属性值。

    d. 在“命名空间”文本框中，键入为该行显示的命名空间值。
    
    e. 单击“确定” 。
    
1. 在“SAML 签名证书”部分中，单击“证书(base64)”，并在计算机上保存证书文件。

    ![证书下载链接](./media/mindwireless-tutorial/tutorial_mindwireless_certificate.png) 

1. 单击“保存”按钮。

    ![配置单一登录“保存”按钮](./media/mindwireless-tutorial/tutorial_general_400.png)

1. 在“mindWireless 配置”部分，单击“配置 mindWireless”打开“配置登录”窗口。 从“快速参考”部分中复制“注销 URL”、“SAML 实体 ID”和“SAML 单一登录服务 URL”。

    ![mindWireless 配置](./media/mindwireless-tutorial/tutorial_mindwireless_configure.png) 

1. 若要在 **mindWireless** 端配置单一登录，需要将下载的“证书(Base64)”、“SAML 单一登录服务 URL”和“SAML 实体 ID”发送给 [mindWireless 支持团队](mailto:sdulloor@mindwireless.com)。 他们会对此进行设置，使两端的 SAML SSO 连接均正确设置。

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

本部分的目的是在 Azure 门户中创建名为 Britta Simon 的测试用户。

   ![创建 Azure AD 测试用户][100]

**若要在 Azure AD 中创建测试用户，请执行以下步骤：**

1. 在 Azure 门户的左窗格中，单击“Azure Active Directory”按钮。

    ![“Azure Active Directory”按钮](./media/mindwireless-tutorial/create_aaduser_01.png)

1. 若要显示用户列表，请转到“用户和组”，然后单击“所有用户”。

    ![“用户和组”以及“所有用户”链接](./media/mindwireless-tutorial/create_aaduser_02.png)

1. 若要打开“用户”对话框，在“所有用户”对话框顶部单击“添加”。

    ![“添加”按钮](./media/mindwireless-tutorial/create_aaduser_03.png)

1. 在“用户”对话框中，执行以下步骤：

    ![“用户”对话框](./media/mindwireless-tutorial/create_aaduser_04.png)

    a. 在“姓名”框中，键入“BrittaSimon”。

    b. 在“用户名”框中，键入用户 Britta Simon 的电子邮件地址。

    c. 选中“显示密码”复选框，然后记下“密码”框中显示的值。

    d. 单击“创建”。

### <a name="create-a-mindwireless-test-user"></a>创建 mindWireless 测试用户

在本部分，我们在 mindWireless 中创建名为 Britta Simon 的用户。 与 [mindWireless 支持团队](mailto:sdulloor@mindwireless.com)合作，在 mindWireless 平台中添加用户。 使用单一登录前，必须先创建并激活用户。 

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分，我们通过授予 Britta Simon 访问 mindWireless 的权限，以支持其使用 Azure 单一登录。

![分配用户角色][200] 

**若要将 Britta Simon 分配到 mindWireless，请执行以下步骤：**

1. 在 Azure 门户中打开应用程序视图，导航到目录视图，接着转到“企业应用程序”，并单击“所有应用程序”。

    ![分配用户][201] 

1. 在应用程序列表中，选择“mindWireless”。

    ![应用程序列表中的 mindWireless 链接](./media/mindwireless-tutorial/tutorial_mindwireless_app.png)  

1. 在左侧菜单中，单击“用户和组”。

    ![“用户和组”链接][202]

1. 单击“添加”按钮。 然后在“添加分配”对话框中选择“用户和组”。

    ![“添加分配”窗格][203]

1. 在“用户和组”对话框的“用户”列表中，选择“Britta Simon”。

1. 在“用户和组”对话框中单击“选择”按钮。

1. 在“添加分配”对话框中单击“分配”按钮。
    
### <a name="test-single-sign-on"></a>测试单一登录

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

单击访问面板中的 mindWireless 磁贴时，应会自动登录到 mindWireless 应用程序。
有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](../user-help/active-directory-saas-access-panel-introduction.md)（访问面板简介）。 

## <a name="additional-resources"></a>其他资源

* [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](tutorial-list.md)
* [什么是使用 Azure Active Directory 的应用程序访问和单一登录？](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/mindwireless-tutorial/tutorial_general_01.png
[2]: ./media/mindwireless-tutorial/tutorial_general_02.png
[3]: ./media/mindwireless-tutorial/tutorial_general_03.png
[4]: ./media/mindwireless-tutorial/tutorial_general_04.png

[100]: ./media/mindwireless-tutorial/tutorial_general_100.png

[200]: ./media/mindwireless-tutorial/tutorial_general_200.png
[201]: ./media/mindwireless-tutorial/tutorial_general_201.png
[202]: ./media/mindwireless-tutorial/tutorial_general_202.png
[203]: ./media/mindwireless-tutorial/tutorial_general_203.png

