---
title: ksetup： addrealmflags
description: 适用于 * * * * 的 Windows 命令主题
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80ca1e16-8871-494b-b9be-6bc9d63de860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21ab18620f0ed25cfb0d50bb1ab1a5d92caa9c54
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841810"
---
# <a name="ksetupaddrealmflags"></a>ksetup： addrealmflags



将其他领域标志添加到指定领域。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
ksetup /addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]
```

#### <a name="parameters"></a>参数

|参数|说明|
|---------|-----------|
|领域名称|领域名称被声明为大写的 DNS 名称，例如 CORP。CONTOSO.COM。|

## <a name="remarks"></a>备注

领域标志指定了不基于 Windows Server 操作系统的 Kerberos 领域的其他功能。 运行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的计算机可以使用 Kerberos 服务器来管理身份验证，而不是使用运行 Windows Server 操作系统的域，这些系统参与了 Kerberos 领域。 此条目将建立领域的功能。 下表对每个进行了说明。

|值|领域标志|说明|
|-----|----------|-----------|
|0xF|全部|设置所有领域标志。|
|0x00|无|未设置领域标志，并且未启用任何其他功能。|
|0x01|SendAddress|此 IP 地址将包含在票证授予票证中。|
|0x02|TcpSupported|此领域支持传输控制协议（TCP）和用户数据报协议（UDP）。|
|0x04|委托|此领域中的每个人都受信任，可用于委派。|
|0x08|NcSupported|此领域支持名称规范化，这允许 DNS 和领域的命名标准。|
|0x80|RC4|此领域支持 RC4 加密以启用跨领域信任，这允许使用 TLS。|

领域标志存储在注册表中的**HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains 下\\** <em>域名</em>。 默认情况下，注册表中不存在此项。 可以使用[Ksetup： addrealmflags](ksetup-addrealmflags.md)命令填充注册表。

可以通过查看 ksetup 或 ksetup/dumpstate. 的输出来了解哪些领域标志可用和设置。

## <a name="examples"></a><a name=BKMK_Examples></a>示例

列出领域 CONTOSO 的可用领域标志：
```
Ksetup /listrealmflags
```
将两个标志设置为 CONTOSO 领域：
```
ksetup /setrealmflags CONTOSO ncsupported delegate
```
添加另一个当前不在该集中的标志：
```
ksetup /addrealmflags CONTOSO SendAddress
```
运行**ksetup**命令，以验证是否已通过查看输出和查找**领域**标志来设置领域标志。

## <a name="additional-references"></a>其他参考

-   [Ksetup:listrealmflags](ksetup-listrealmflags.md)
-   [Ksetup:setrealmflags](ksetup-setrealmflags.md)
-   [Ksetup:delrealmflags](ksetup-delrealmflags.md)
-   - [命令行语法项](command-line-syntax-key.md)