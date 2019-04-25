---
title: San
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d57c2df1-eb82-4b81-b8cd-e30564c6a929
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a999254507de2d90494c1acfb906d4bcf26168aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872498"
---
# <a name="san"></a>San

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示或设置存储区域网络 (san) 策略的操作系统。
> [!NOTE]
> 此命令当前仅适用于 Windows 7 和 Windows Server 2008 R2。

## <a name="syntax"></a>语法
```
san [policy={onlineAll | offlineAll | offlineShared}] [noerr]
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|策略 = {onlineAll &#124; offlineAll&#124;离线共享}]|设置当前已启动的操作系统的 san 策略。 San 策略确定新发现的磁盘是否联机或将保持脱机状态，以及它将成为读/写还是保持只读的。 当磁盘处于脱机状态时，可以读取的磁盘布局，但没有卷的设备会显示通过插。 这意味着没有文件系统，可以装载在磁盘上。 当磁盘处于联机状态时，为磁盘安装一个或多个卷设备。 下面是每个参数的说明：<br /><br />-   **onlineAll**。 指定所有新发现的磁盘都将置于联机状态并且会成为读/写。 **重要：**   指定**onlineAll**共享磁盘的服务器上可能会导致数据损坏。 因此，如果磁盘在服务器之间共享，除非服务器是群集的一部分，不应设置此策略。<br />-   **offlineAll**。 指定所有新发现的磁盘，除了启动磁盘将处于脱机状态仅 andread 默认情况下。<br />-   **离线共享**。 指定所有新发现的不是驻留在共享总线上 （如 SCSI 和 iSCSI） 的磁盘恢复为联机和所做的只写。 默认情况下以只读方式将会保持脱机状态的磁盘。<br /><br />有关详细信息，请参阅[VDS_san_POLICY 枚举](https://go.microsoft.com/fwlink/?LinkId=203815)(https://go.microsoft.com/fwlink/?LinkId=203815)。|
|noerr|用于仅编写脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。|
## <a name="remarks"></a>备注
-   如果该命令指定不带任何参数，将显示当前的 san 策略。
## <a name="BKMK_Examples"></a>示例
若要查看当前的策略，请键入：
```
san
```
若要使所有新发现的磁盘，启动磁盘除外，离线，默认情况下以只读的请键入：
```
san policy=offlineAll
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)