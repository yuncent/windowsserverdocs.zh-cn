---
title: unlodctr
description: 用于 unlodctr 的 Windows 命令主题，用于从系统注册表中删除服务或设备驱动程序的性能计数器名称和说明文本
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe7fc3c9eafefd59a5daab625e3af06b6addd292
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832254"
---
# <a name="unlodctr"></a>unlodctr

>适用于：Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

从系统注册表中删除服务或设备驱动程序的**性能计数器**名称和**说明**文本。   

## <a name="syntax"></a>语法  
```  
Unlodctr <DriverName>   
```  
#### <a name="parameters"></a>参数  
|参数|说明|  
|-------|--------|  
|\<DriverName >|从 Windows Server 2003 注册表删除驱动程序或服务 <DriverName> 的性能计数器名称设置和说明文本。|  
|/?|在命令提示符下显示帮助。|  

## <a name="remarks"></a>备注  
> [!WARNING]  
> 错误编辑注册表可能会严重损坏您的系统。 在更改注册表之前，应备份计算机上任何有价值的数据。  

如果提供的信息包含空格，请使用引号将文本括起来（例如 <DriverName>）。  

## <a name="examples"></a><a name=BKMK_Examples></a>示例  
若要为简单邮件传输协议（SMTP）服务删除当前性能注册表设置和计数器说明文本，请执行以下操作：  
```  
unlodctr SMTPSVC  
```  
## <a name="additional-references"></a>其他参考  
-   - [命令行语法项](command-line-syntax-key.md)  
  
