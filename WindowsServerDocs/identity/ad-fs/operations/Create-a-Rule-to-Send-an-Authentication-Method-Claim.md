---
ms.assetid: 96b9f4e6-f01c-4517-8299-017d187d447e
title: 创建规则以发送身份验证方法声明
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2c011c3b15f6223a6cea2f9c7226d9319641a693
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816571"
---
# <a name="create-a-rule-to-send-an-authentication-method-claim"></a>创建规则以发送身份验证方法声明


你可以使用 "**以声明方式发送组成员身份**" 规则模板或 "**转换传入声明**" 规则模板来发送身份验证方法声明。 依赖方可以使用身份验证方法声明来确定登录机制，用户使用该机制进行身份验证并获取 Active Directory 联合身份验证服务 \(AD FS\)中的声明。 你还可以使用 Active Directory 联合身份验证服务的身份验证机制保证功能，将 Windows Server 2012 R2 中 \(AD FS\) 作为输入，以生成身份验证方法声明，以便信赖方希望确定基于智能卡登录的访问级别。 例如，开发人员可为信赖方应用程序的联合用户分配不同级别的访问权限。 访问级别取决于用户是否使用其用户名和密码凭据进行登录，而不是使用其智能卡。  

根据组织的要求，请使用以下过程之一：  

-   如果希望在此模板中指定的组最终确定要颁发的身份验证方法声明，请使用 "**以声明方式发送组成员身份**" 规则模板来创建此规则 \- 你可以使用此规则模板。  

-   使用 "**转换传入声明**" 规则模板来创建此规则 \- 如果要将现有的身份验证方法更改为与无法识别标准 AD FS 身份验证方法声明的产品一起使用的新身份验证方法，则可以使用此规则模板。  



## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>使用 Windows Server 2016 中信赖方信任上的 "以声明方式发送组成员身份" 规则模板来创建 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  右键\-单击选定的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**发送组成员身份作为声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.  在 "**配置规则**" 页上，键入声明规则名称。  

7.  单击 "**浏览**"，选择其成员应接收此身份验证方法声明的组，然后单击 **"确定"** 。  

8.  在 "**传出声明类型**" 中，选择列表中的 "**身份验证方法**"。  

9. 在 "**传出声明值**" 中，键入下表中 \(URI 的默认统一资源标识符之一\) 值，具体取决于你的首选身份验证方法，请单击 "**完成**"，然后单击 **"确定"** 保存规则。  

|                            实际身份验证方法                             |                                对应 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        用户名和密码身份验证                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 身份验证                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 传输层安全 \(TLS\) 使用 x.509 证书的相互身份验证 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  不使用 TLS 的基于 x.509\-的身份验证                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)

## <a name="to-create-by-using-the-send-group-membership-as-claims-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>通过在 Windows Server 2016 中的声明提供方信任上使用 "以声明方式发送组成员身份" 规则模板来创建 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  右键\-单击选定的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**发送组成员身份作为声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.  在 "**配置规则**" 页上，键入声明规则名称。  

7.  单击 "**浏览**"，选择其成员应接收此身份验证方法声明的组，然后单击 **"确定"** 。  

8.  在 "**传出声明类型**" 中，选择列表中的 "**身份验证方法**"。  

9. 在 "**传出声明值**" 中，键入下表中 \(URI 的默认统一资源标识符之一\) 值，具体取决于你的首选身份验证方法，请单击 "**完成**"，然后单击 **"确定"** 保存规则。  

|                            实际身份验证方法                             |                                对应 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        用户名和密码身份验证                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 身份验证                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 传输层安全 \(TLS\) 使用 x.509 证书的相互身份验证 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  不使用 TLS 的基于 x.509\-的身份验证                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth2.PNG)


## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>使用在 Windows Server 2016 中的信赖方信任上转换传入声明规则模板来创建此规则 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  

3.  右键\-单击选定的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   

4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**转换传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 "**配置规则**" 页上，键入声明规则名称。  

7.  在 "**传入声明类型**" 中，选择列表中的 "**身份验证方法**"。  

8.  在 "**传出声明类型**" 中，选择列表中的 "**身份验证方法**"。  

9. 选择 "将**传入声明值替换为不同的传出声明值**"，然后执行以下操作：  

    1.  在 "**传入声明值**" 中，键入以下基于最初使用的实际身份验证方法 URI 的 uri 值之一，单击 "**完成**"，然后单击 **"确定"** 以保存该规则。  

    2.  在 "**传出声明值**" 中，键入下表中的一个默认 URI 值（取决于新的首选身份验证方法选择），单击 "**完成**"，然后单击 **"确定"** 保存规则。  

|              实际身份验证方法              |                                对应 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         用户名和密码身份验证          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 身份验证                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 证书的 TLS 相互身份验证 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   不使用 TLS 的基于 x.509\-的身份验证    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)

> [!NOTE]  
> 除了表中的值之外，还可以使用其他 URI 值。 上表中显示的 URI 值反映了依赖方默认接受的 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>使用在 Windows Server 2016 中的声明提供方信任上转换传入声明规则模板来创建此规则 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  

3.  右键\-单击选定的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   

4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**转换传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 "**配置规则**" 页上，键入声明规则名称。  

7.  在 "**传入声明类型**" 中，选择列表中的 "**身份验证方法**"。  

8.  在 "**传出声明类型**" 中，选择列表中的 "**身份验证方法**"。  

9. 选择 "将**传入声明值替换为不同的传出声明值**"，然后执行以下操作：  

    1.  在 "**传入声明值**" 中，键入以下基于最初使用的实际身份验证方法 URI 的 uri 值之一，单击 "**完成**"，然后单击 **"确定"** 以保存该规则。  

    2.  在 "**传出声明值**" 中，键入下表中的一个默认 URI 值（取决于新的首选身份验证方法选择），单击 "**完成**"，然后单击 **"确定"** 保存规则。  

|              实际身份验证方法              |                                对应 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         用户名和密码身份验证          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 身份验证                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 证书的 TLS 相互身份验证 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   不使用 TLS 的基于 x.509\-的身份验证    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth4.PNG)























## <a name="to-create-this-rule-by-using-the-send-group-membership-as-claims-rule-template-in-windows-server-2012-r2"></a>使用 Windows Server 2012 R2 中的 "以声明方式发送组成员身份" 规则模板来创建此规则  

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD FS\\信任关系**" 下，单击 "**声明提供方**信任或**信赖方信任**"，然后在要创建此规则的列表中单击特定信任。  

3.  右键\-单击选定的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  

4.  在 "**编辑声明规则**" 对话框中，根据所编辑的信任和要在其中创建此规则的规则集，选择下列选项卡之一，然后单击 "**添加规则**" 以启动与该规则集关联的规则向导：  

    -   **接受转换规则**  

    -   **颁发转换规则**  

    -   **颁发授权规则**  

    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**以声明方式发送组成员身份**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  在 "**配置规则**" 页上，键入声明规则名称。  

7.  单击 "**浏览**"，选择其成员应接收此身份验证方法声明的组，然后单击 **"确定"** 。  

8.  在 "**传出声明类型**" 中，选择列表中的 "**身份验证方法**"。  

9. 在 "**传出声明值**" 中，键入下表中 \(URI 的默认统一资源标识符之一\) 值，具体取决于你的首选身份验证方法，请单击 "**完成**"，然后单击 **"确定"** 保存规则。  

|                            实际身份验证方法                             |                                对应 URI                                 |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|
|                        用户名和密码身份验证                        | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                               Windows 身份验证                                |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 传输层安全 \(TLS\) 使用 x.509 证书的相互身份验证 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|                  不使用 TLS 的基于 x.509\-的身份验证                  |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth1.PNG)

> [!NOTE]  
> 除了表中的值之外，还可以使用其他 URI 值。 上表中显示的 URI 值反映了信赖方默认接受的 Uri。  

## <a name="to-create-this-rule-by-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>使用 Windows Server 2012 R2 中的 "转换传入声明" 规则模板来创建此规则  



1.  在服务器管理器中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  

2.  在控制台树中的 " **AD FS\\信任关系**" 下，单击 "**声明提供方**信任或**信赖方信任**"，然后在要创建此规则的列表中单击特定信任。  

3.  右键\-单击选定的信任，然后单击 "**编辑声明规则**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 

4.  在 "**编辑声明规则**" 对话框中，选择下列选项卡，其中一个选项卡依赖于你正在编辑的信任和你要在哪个规则集中创建此规则，然后单击 "**添加规则**" 以启动与该规则集关联的规则向导：  

    -   **接受转换规则**  

    -   **颁发转换规则**  

    -   **颁发授权规则**  

    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**转换传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)    

6.  在 "**配置规则**" 页上，键入声明规则名称。  

7.  在 "**传入声明类型**" 中，选择列表中的 "**身份验证方法**"。  

8.  在 "**传出声明类型**" 中，选择列表中的 "**身份验证方法**"。  

9. 选择 "将**传入声明值替换为不同的传出声明值**"，然后执行以下操作：  

    1.  在 "**传入声明值**" 中，键入以下基于最初使用的实际身份验证方法 URI 的 uri 值之一，单击 "**完成**"，然后单击 **"确定"** 以保存该规则。  

    2.  在 "**传出声明值**" 中，键入下表中的一个默认 URI 值（取决于新的首选身份验证方法选择），单击 "**完成**"，然后单击 **"确定"** 保存规则。  

|              实际身份验证方法              |                                对应 URI                                 |
|--------------------------------------------------------|----------------------------------------------------------------------------------|
|         用户名和密码身份验证          | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password  |
|                 Windows 身份验证                 |  https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows  |
| 使用 x.509 证书的 TLS 相互身份验证 | https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/tlsclient |
|   不使用 TLS 的基于 x.509\-的身份验证    |   https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/x509    |

![创建规则](media/Create-a-Rule-to-Send-an-Authentication-Method-Claim/auth3.PNG)

> [!NOTE]  
> 除了表中的值之外，还可以使用其他 URI 值。 上表中显示的 URI 值反映了依赖方默认接受的 Uri。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  

[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  

[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  

[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
