---
title: Vssadmin list shadows
description: Vssadmin list shadows 命令的说明。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: cadbacb5225e28118ec71e9cbad6b3c57a6086e0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830060"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin list shadows

>适用于： Windows 10，Windows 8.1，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server 2008

列出指定卷的所有现有卷影副本。 如果使用不带参数的此命令，则会按**卷影副本集**所指示的顺序显示计算机上的所有卷影副本。

## <a name="syntax"></a>语法

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>参数

|参数|说明|
|---|---|
|/for =\<ForVolumeSpec >|指定将列出卷影副本的卷。|
|/shadow =\<ShadowID >|列出 ShadowID 指定的卷影副本。 若要获取卷影副本 ID，请使用**vssadmin list shadows**命令。 键入卷影副本 ID 时，请使用以下格式，其中每个*X*表示十六进制字符：<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX （XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX）|

## <a name="additional-references"></a>其他参考

* [命令行语法关键字](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [List](vssadmin.md)