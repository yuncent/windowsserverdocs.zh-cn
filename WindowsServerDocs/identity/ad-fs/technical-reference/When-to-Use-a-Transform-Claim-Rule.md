---
ms.assetid: 77aa61bf-9c04-4889-a5d2-6f45bc1b8bd2
title: 何时使用转换声明规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c7b7ea2c8d9a08a4cbf6c89c2de2482043efe25b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885558"
---
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="when-to-use-a-transform-claim-rule"></a>何时使用转换声明规则
可以在 Active Directory 联合身份验证服务中使用此规则\(AD FS\)何时需要将传入声明类型映射到传出声明类型，然后应用一项操作，以确定应出现的输出基于值的源于传入声明中。 使用此规则时，将根据你在此规则中配置的任一选项传递或转换与以下规则逻辑匹配的声明，如下表所述。  
  
|规则选项|规则逻辑|  
|---------------|--------------|  
|传递所有传入声明|如果传入声明类型等于*指定的声明类型* 并且值等于*任意值*，则传递该声明并且传出声明类型等于*指定的声明类型*|  
|将传入声明值替换为不同的传出声明值|如果传入声明类型等于*指定的声明类型* 并且值等于*指定的声明值*，则使用新的传出声明值*指定的声明值* 和传出声明类型*指定的声明类型* 转换该声明。|  
|替换为传入电子\-邮件后缀声明与新的电子\-邮件后缀|如果传入声明类型等于*指定的声明类型* 并且值等于*任意后缀值*，则使用新的传出声明值*指定的后缀值* 和传出声明类型*指定的声明类型* 转换该声明。|  
  
以下部分提供了声明规则的基本介绍，并提供了有关何时使用此规则的更多详细信息。  
  
## <a name="about-claim-rules"></a>关于声明规则  
声明规则表示业务逻辑，使用传入声明、 向它应用条件的实例\(if x，then y\)和生成传出声明基于条件参数。 下面的列表概述了在进一步阅读本主题中的内容之前应了解的有关声明规则的重要提示：  
  
-   在 AD FS 管理管理单元\-中，声明只可以使用声明规则模板创建规则  
  
-   声明规则过程传入声明既可以直接从声明提供程序\(Active Directory 或另一个联合身份验证服务等\)或输出中的接受转换规则声明提供方信任上的。  
  
-   声明规则由声明颁发引擎按给定规则集内的时间顺序处理。 通过为规则设置优先级，可以进一步优化或筛选由给定规则集内以前的规则生成的声明。  
  
-   声明规则模板始终要求你指定传入声明类型。 但是，你可以使用单个规则处理声明类型相同的多个声明值。  
  
有关详细信息有关声明规则和声明规则集的详细信息，请参阅[规则的角色声明](The-Role-of-Claim-Rules.md)。 有关如何处理规则的详细信息，请参阅[The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。 有关如何处理声明规则集的详细信息，请参阅[声明管道的角色](The-Role-of-the-Claims-Pipeline.md)。  
  
## <a name="pass-through-all-claim-values"></a>传递所有声明值  
使用此操作时，会将与指定的传入声明类型相匹配的所有传入声明值映射到指定的传出声明类型，然后将这些声明值作为传出声明发送到由联合身份验证服务签名的令牌中。  
  
例如，如果为规则设置“传递所有声明值”选项逻辑并且指定传入声明类型“组”和传出声明类型“角色”，那么，从发出方流入的所有传入声明值将逐一复制到声明类型为“角色”的新传出声明中。  
  
## <a name="transforming-a-claim"></a>转换声明  
在 AD FS 中，术语*声明转换*方法替换为一个传入声明值，该值具有不同的传出声明值。 “转换传入声明”规则使该功能变为可能。 在该规则的属性内，你可以设置条件，以便根据指定的传入声明类型将传入值转换为不同的传出声明值。  
  
例如，如下图所示，如果为规则设置将传入值替换为不同的传出声明值的条件，所有传入声明类型“组”都将映射到新的传出声明类型“角色”。 在这种情况下，传入声明值“Purchaser”将替换为新的传出声明值“Admin”。  
  
![何时使用转换](media/adfs2_transform.gif)  
  
此外可以使用此规则要应用条件，会将所有传入声明指定 e 替换为\-邮件后缀值与新值。 例如，你可以在此规则中设置一个条件，以便将所有声明值的后缀 sales.corp.fabrikam.com 更改为 fabrikam.com。  
  
## <a name="configuring-this-rule-on-a-claims-provider-trust"></a>对声明提供程序信任配置此规则  
使用声明提供程序信任时，可以将此规则配置为将来自声明提供程序的传入声明转换为可信的等效声明。 声明类型或声明值在你的组织中的含义与在声明提供程序组织中的含义可能有所不同。 你可以使用此规则对来自声明提供程序的声明类型和值执行标准化，以便于信赖方理解其传出声明类型和值。  
  
## <a name="configuring-this-rule-on-a-relying-party-trust"></a>对信赖方信任配置此规则  
使用信赖方信任时，可以将此规则配置为针对特定信赖方转换声明。 声明类型或声明值对于特定的信赖方可能有不同的含义，此规则允许为单个信赖方更改传出声明类型和值。  
  
## <a name="how-to-create-this-rule"></a>如何创建此规则  
创建此规则使用声明规则语言或使用**转换传入声明**AD FS 管理管理单元中的规则模板\-中。 此规则模板提供以下配置选项：  
  
-   指定声明规则名称  
  
-   将特定的传入声明类型转换为指定的传出声明类型  
  
-   传递所有声明值  
  
-   将传入声明值替换为不同的传出声明值  
  
-   替换为传入电子\-邮件后缀声明与新的电子\-邮件后缀  
  
有关如何创建此模板的详细说明，请参阅[创建一个规则以转换传入声明](https://technet.microsoft.com/library/dd807068.aspx)AD FS 部署指南中。  
  
## <a name="using-the-claim-rule-language"></a>使用声明规则语言  
如果传出声明必须根据多个传入声明的内容构造，则必须改为使用自定义规则。 如果传出声明的声明值必须基于传入声明的值，但是还包括其他内容，那么，也必须在该上下文中使用自定义规则。 有关详细信息，请参阅 [When to Use a Custom Claim Rule](When-to-Use-a-Custom-Claim-Rule.md)。  
  
### <a name="examples-of-how-to-construct-a-transform-rule-syntax"></a>有关如何构造转换规则语法的示例  
使用声明规则语言语法转换声明时，可以将已转换声明的属性设置为新的文本值。 例如，下面的规则将角色声明的值从“Administrators”更改为“root”，同时保持相同的声明类型：  
  
```  
c:[type == “https://schemas.microsoft.com/ws/2008/06/identity/claims/role”, value == “Administrators”]  => issue(type = c.type, value = “root”);  
```  
  
正则表达式也可用于声明转换。 例如，以下规则将在域中设置的 windows 用户名声明在域中\\到 FABRIKAM 的用户格式：  
  
```  
c:[type == "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"] => issue(type = c.type, value = regexreplace(c.value, "(?<domain>[^\\]+)\\(?<user>.+)", "FABRIKAM\${user}"));  
```  
  
### <a name="best-practices-for-creating-custom-rules"></a>创建自定义规则的最佳做法  
可以向使用基本筛选功能选定的声明有选择性地应用声明转换。 可以为用于筛选的每个声明属性分配值，并提供以下说明：  
  
|声明属性|描述|  
|------------------|---------------|  
|Type、Value、ValueType|为这些属性分配值的频率最高。 必须为生成的已转换声明至少指定类型和值。|  
|颁发者|虽然声明规则语言允许设置声明的发出方，但通常不建议这样做。 声明的发出方在令牌中不会序列化。 收到令牌时，所有声明的 Issuer 属性都将设置为已对该令牌签名的联合服务器的标识符。 因此，在规则中设置声明的发出方对令牌的内容没有任何影响，而且一旦在令牌中打包该声明，该设置便会丢失。 唯一需要设置声明发出方的情况为：它在声明提供程序规则集中设置为某个特定值，而信赖方规则集使用引用此特定值的规则创作。 如果声明规则中的 Issuer 属性未显式设置为某个值，声明发出引擎会将其设置为“本地机构”。|  
|OriginalIssuer|与 Issuer 相同的是，OriginalIssuer 通常不应显式分配一个值。 与 Issuer 不同的是，OriginalIssuer 属性在令牌中会被序列化，但令牌使用者所希望的是，如果设置，它将包含最初发出声明的联合服务器的标识符。|  
|属性|如前一部分所述，声明的属性包不会保存在令牌中，因此，应该在后续的本地策略要引用属性中存储的信息时，才对属性分配值。|  
  
有关如何使用声明规则语言的详细信息，请参阅[的声明规则语言角色](The-Role-of-the-Claim-Rule-Language.md)。  
  
