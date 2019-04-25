---
title: 将 GUID 分区表 (GPT) 磁盘更改为主启动记录 (MBR) 磁盘
description: 介绍如何将 GUID 分区表 (GPT) 磁盘更改为主启动记录 (MBR) 分区样式磁盘。
ms.date: 06/19/2018
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ae755b9c41d66ce5f907f600be17547398acc1a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839578"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>将 GPT 磁盘转换为 MBR 磁盘

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

主启动记录 (MBR) 磁盘使用标准 BIOS 分区表。 GUID 分区表 (GPT) 磁盘使用统一可扩展固件接口 (UEFI)。 MBR 磁盘不支持每个磁盘上超过四个分区。 对于大于 2 TB 的磁盘，不建议使用 MBR 分区方法。

只要磁盘为空并且未包含任何卷，就可以将磁盘从 GPT 更改为 MBR 分区样式。

> [!NOTE]
> 转换磁盘之前，备份上它的任何数据，并关闭任何正在访问该磁盘的程序。

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

<a id="BKMK_WINUI"></a>

## <a name="converting-using-the-windows-interface"></a>转换使用 Windows 界面

1.  备份或移动你想要转换为 MBR 磁盘的基本 GPT 磁盘上的所有卷。

2.  如果该磁盘包含任何分区或卷，请右键单击每一项，然后单击**删除卷**。

3.  右键单击你想要更改为 MBR 磁盘的 GPT 磁盘，然后单击**转换成 MBR 磁盘**。

<a id="BKMK_CMD"></a>

## <a name="converting-using-a-command-line"></a>转换使用命令行

1.  备份或移动你想要转换为 MBR 磁盘的基本 GPT 磁盘上的所有卷。

2.  右键单击**命令提示符**，然后选择**以管理员身份运行**，打开提升的命令提示符。

3. 键入 `diskpart`。 如果该磁盘未包含分区或卷，请跳到步骤 6。

4.  在 **DISKPART** 提示符下，键入 `list disk`。 请记下要删除的磁盘编号。

5.  在 **DISKPART** 提示符下，键入 `select disk <disknumber>`。

6.  在 **DISKPART** 提示符下，键入 `clean`。

    > [!IMPORTANT]
    > 运行 **clean** 命令将删除磁盘上的所有分区或卷。

7.  在 **DISKPART** 提示符下，键入 `convert mbr`。

<br />

| ReplTest1 | 描述 |
| --- | --- |
| <p>**list disk**</p> | <p>显示磁盘列表和有关磁盘的信息，例如磁盘大小、可用空间量、磁盘是基本磁盘还是动态磁盘，以及磁盘是使用主启动记录 (MBR) 还是 GUID 分区表 (GPT) 分区样式。 用星号 (*) 标记的磁盘具有焦点。</p> |
| <p>**选择磁盘**</p> | <p>选择指定的磁盘（其中 <em>disknumber</em> 是磁盘编号），并赋予其焦点。</p> | <p>**clean**</p> | <p>从具有焦点的磁盘中删除所有分区或卷。</p> |
| <p>**将 mbr 转换**</p> | <p>将具有 GUID 分区表 (GPT) 分区样式的空基本磁盘转换为具有主启动记录 (MBR) 分区样式的基本磁盘。</p>

## <a name="see-also"></a>请参阅

-   [命令行语法表示法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)

