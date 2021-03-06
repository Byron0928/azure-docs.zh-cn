---
title: include 文件
description: include 文件
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 07/27/2018
ms.openlocfilehash: 12f0f17a7c25696d2c8ff5b427f4b103617e5678
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2018
ms.locfileid: "47010710"
---
在命令提示符或 shell 窗口中，使用 numpy 和 cython 创建并激活 conda 包管理器环境。 本示例使用 Python 3.6。

  + 在 Windows 上：
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       conda activate myenv
       ```

  + 在 Linux 或 MacOS 上：
       ```sh 
       conda create -n myenv Python=3.6 cython numpy
       source activate myenv
       ```

安装 SDK。
   ```sh 
   pip install azureml-sdk
   ```
