---
title: PVLAN 配置虚拟交换机上的必须保持一致
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b7d6d068027aa9497b00138bd1d889ea86aa3308
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813248"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>PVLAN 配置虚拟交换机上的必须保持一致

>适用于：Windows Server 2016

有关最佳做法和扫描的详细信息，请参阅[运行最佳做法分析器扫描并管理扫描结果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016| 
|**产品/功能**|Hyper-V|  
|**Severity**|错误|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。
  
## <a name="issue"></a>**问题**  
*专用虚拟局域网 (PVLAN) 对一个或多个虚拟网络适配器的配置不正确。*  
  
## <a name="impact"></a>**影响**  
*PVLAN 可能不正确隔离的虚拟机之间的网络流量。*  
  
## <a name="resolution"></a>**解决方法**  
*使用 Windows PowerShell cmdlet，Set-vmnetworkadaptervlan，若要正确配置 PVLAN。*  
  

