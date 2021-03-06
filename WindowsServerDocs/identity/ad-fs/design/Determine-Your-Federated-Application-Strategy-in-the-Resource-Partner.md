---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: 确定资源伙伴中的联合应用程序策略
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b7b50d93ef09259cd6d1893eda4fd5c651b8e688
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853150"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>确定资源伙伴中的联合应用程序策略

在资源伙伴组织中设计新的 Active Directory 联合身份验证服务 \(AD FS\) 基础结构的一个重要部分是确定将用于参与联合的完整应用程序和服务集，以及哪些帐户伙伴将作为这些资源的接收方。 设计联合应用程序和服务策略之前，请考虑以下问题：  
  
-   是否要为联合启用和部署 ASP.NET 应用程序或 Windows Communication Foundation \(WCF\) 服务？  
  
-   公司网络上的用户是否需要通过 Windows 集成身份验证访问联合应用程序或服务？  
  
-   联合应用程序或服务是否会由外围网络中的用户使用？ 如果是这样，是否需要 Windows 集成身份验证？  
  
-   承载联合应用程序的所有 Web 服务器是否都在运行 Windows Server 操作系统，并 Internet Information Services \(IIS\)？  
  
-   联合应用程序或服务为谁提供资源？  
  
回答这些问题将有助于规划坚实的 AD FS 设计。 它还可帮助创建具有成本效益且在资源方面十分高效的联合应用程序和服务策略。 有关为你的组织设计最合适的联合应用程序和服务策略的详细信息，请参阅本指南中的以下主题：  
  
-   [为声明感知应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [为其他组织的应用程序和服务提供 Active Directory 用户访问权限](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [为另一个组织中的用户提供对声明感知应用程序和服务的访问权限](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
若要详细了解如何创建\-感知 ASP.NET 应用程序或 WCF 服务的声明，请参阅[Windows Identity FOUNDATION SDK](https://go.microsoft.com/fwlink/?LinkId=122266)。  
  
## <a name="see-also"></a>另请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)

