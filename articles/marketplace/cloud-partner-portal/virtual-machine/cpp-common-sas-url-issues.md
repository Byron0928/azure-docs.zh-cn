---
title: Azure 市场中的常见 SAS URL 问题和修复 | Microsoft Docs
description: 列出了使用共享访问签名 URI 时的常见问题和可能的解决方案。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/27/2018
ms.author: pbutlerm
ms.openlocfilehash: b20b1506dfcd32ea7d5bfca0847393d1652afb78
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638886"
---
# <a name="common-sas-url-issues-and-fixes"></a>常见的 SAS URL 问题和修复

下表列出了使用共享访问签名（它们用来标识和共享解决方案的已上传 VHD）时遇到的一些常见问题以及建议的解决方法。

| **问题** | **失败消息** | **修复** | 
| --------- | ------------------- | ------- | 
| &emsp;  *复制映像时失败* |  |  |  |
| 在 SAS URL中未找到 "?" | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 使用建议的工具更新 SAS URL。 |
| SAS URL 中不存在“st”和“se”参数 | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 更新 SAS URL，在其中设置正确的**开始日期**和**结束日期**值。 | 
| SAS URL 中不存在“sp=rl” | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | 更新 SAS URL，将权限设置为 `Read` 和 `List`。 | 
| SAS URL 中的 VHD 名称中存在空格 | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | 更新 SAS URL 以删除空格。 |
| SAS URL 授权错误 | `Failure: Copying Images. Not able to download blob due to authorization error` | 检查并更正 SAS URI 格式。 必要时重新生成。 |
| SAS URL“st”和“se”参数没有指定完整的日期时间 | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS URL **开始日期**和**结束日期**参数（`st` 和 ` se` 子字符串）必须具有完整的日期时间格式，例如 `11-02-2017T00:00:00Z`。 缩短的版本无效。 （默认情况下，Azure CLI 中的某些命令可能会生成缩短的值。） | 
|  |  |  |

有关详细信息，请参阅[使用共享访问签名 (SAS)](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)。
