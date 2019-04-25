---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: 为 AD FS 配置额外的身份验证方法
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73de8908677b3f74651b10c29ef2abe62e484694
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868408"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>为 AD FS 配置额外的身份验证方法

>适用于：Windows Server 2016, Windows Server 2012 R2

若要启用多因素身份验证 (MFA)，你必须至少选择一种附加身份验证方法。 默认情况下，在 Windows Server 2012 R2 中，Active Directory 联合身份验证服务 (AD FS) 中您可以选择证书身份验证 （即，基于智能卡的身份验证） 作为附加身份验证方法。

> [!NOTE]
> 如果选择证书身份验证，请确保智能卡证书已安全地进行设置并且指定了 PIN 码要求。

你是否知道 Microsoft Azure 在云中提供类似的功能？ 了解有关 [Microsoft Azure 标识解决方案](http://aka.ms/m2w274)的详细信息。<br /><br />在 Microsoft Azure 中创建混合标识解决方案：<br /> - [了解有关 Azure 多重身份验证信息。](http://aka.ms/ey6o9r)<br /> - [管理单个林混合环境使用云身份验证。](http://aka.ms/g1jat8)<br /> - [管理使用适用于敏感应用程序的附加多重身份验证的风险。](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Microsoft 和第三方附加身份验证方法
此外可以配置和启用 Microsoft 和 Windows Server 2012 R2 中的 AD FS 中的第三方身份验证方法。 一旦安装并注册到 AD FS，你可以强制执行 MFA 作为全局或每个信赖方身份验证策略的一部分。

下面是按字母顺序排列的 Microsoft 和第三方提供商以及当前可用于 Windows Server 2012 R2 中的 AD FS 的 MFA 产品/服务的列表。

|提供程序|产品/服务|用于了解详细信息的链接|
|-|-|-| 
|aPersona|aPersona Microsoft ADFS sso 的自适应多因素身份验证|[aPersona ASM ADFS 适配器](https://www.apersona.com/adfs)|
|双重安全性|适用于 AD FS duo MFA 适配器|[适用于 AD FS duo 身份验证](https://duo.com/docs/adfs)|
|Gemalto|Gemalto 身份标识和安全服务|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo Technologies|inWebo 企业身份验证服务|[inWebo 企业身份验证](http://www.inwebo.com)|
|Login People|适用于 AD FS 2012 R2 的 Login People MFA API 连接程序（公用 Beta 版）|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[操作实例指南：使用适用于敏感应用程序的附加多因素身份验证管理风险](https://technet.microsoft.com/library/dn280946.aspx)（请参阅步骤 3）|
Mideye | ADFS 的 mideye 身份验证提供程序 | [使用 Microsoft Active Directory 联合身份验证服务的 mideye 双因素身份验证](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Active Directory 联合身份验证服务的 Okta MFA | [Okta MFA 的 Active Directory 联合身份验证服务 (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|一个标识| Starling 2FA AD FS|[Starling 2FA AD FS 适配器](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|一个标识| Defender AD FS|[Defender AD FS 适配器](https://www.oneidentity.com/products/defender/)|
|Ping 标识|适用于 AD FS PingID MFA 适配器|[适用于 AD FS PingID MFA 适配器](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, The Security Division of EMC|适用于 Microsoft Active Directory 联合身份验证服务的 RSA SecurID 身份验证代理|[Microsoft Active Directory 联合身份验证服务的 RSA SecurID 身份验证代理](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|适用于 AD FS 的 SafeNet 身份验证服务 (SAS) 代理|[SafeNet 身份验证服务：AD FS 代理配置指南](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|SecureMFA OTP 提供程序| [ADFS 多重身份验证提供程序](https://www.securemfa.com/)|
|Swisscom|移动设备 ID 身份验证服务和签名服务|[移动设备 ID 身份验证服务](http://swisscom.ch/mid)|
|Symantec|Symantec 验证和 ID 保护服务 (VIP)|[Symantec 验证和 ID 保护服务 (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|基本 (无密码 MFA) 和高级管理人员 （基本 + 标识校对）| [Trusona 多重身份验证](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>适用于 Windows Server 2012 R2 中 AD FS 的自定义身份验证方法
现在，我们将提供针对 Windows Server 2012 R2 中的 AD FS 自行构建自定义身份验证方法的相关说明。 有关详细信息，请参阅 [针对 Windows Server 2012 R2 中的 AD FS 构建自定义身份验证方法](https://go.microsoft.com/fwlink/?LinkID=511980)。

## <a name="see-also"></a>请参阅
[使用针对敏感应用程序的附加多重身份验证管理风险](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

