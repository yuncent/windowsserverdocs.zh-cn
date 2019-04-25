---
title: 将现有的文件服务器配置为内容服务器
description: 本主题是 BranchCache 部署指南为 Windows Server 2016 中，该示例演示了如何部署 BranchCache 在分布式和托管缓存模式下以优化分支机构中的 WAN 带宽使用情况的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d031e8aa853849c322692552ca9107838cebb5e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866908"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>将现有的文件服务器配置为内容服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用此过程来安装**网络文件 BranchCache**运行 Windows Server 2016 的计算机上的文件服务服务器角色的角色服务。  
  
> [!IMPORTANT]  
> 如果尚未安装文件服务服务器角色，则不会遵循此过程。 相反，请参阅[将新的文件服务器安装为内容服务器](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md)。  
  
中的成员身份**管理员**，或等效身份是执行此过程所需的最低。  
  
> [!NOTE]  
> 若要使用 Windows PowerShell，以管理员身份运行 Windows PowerShell 执行此过程在 Windows PowerShell 提示符下键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> 若要安装重复数据删除角色服务，键入以下命令，然后按 ENTER。  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>若要安装网络文件 BranchCache 角色服务  
  
1.  在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。 添加角色和功能向导将打开。 单击“下一步” 。  
  
2.  在中**选择安装类型**，确保**基于角色或基于功能的安装**已选择，然后单击**下一步**。  
  
3.  在中**选择目标服务器**，确保正确的服务器选择，然后单击**下一步**。  
  
4.  在中**选择服务器角色**，在**角色**，请注意，**文件和存储服务**已安装角色; 单击角色名称以展开左侧的箭头选择角色服务，然后单击左侧的箭头**文件和 iSCSI 服务**。  
  
5.  选中的复选框**网络文件 BranchCache**。  
  
    > [!TIP]  
    > 如果尚未这样做，则建议你还选择复选框**重复数据删除**。  
  
    单击“下一步” 。  
  
6.  在中**选择的功能**，单击**下一步**。  
  
7.  在中**确认安装选择**，查看所选内容，然后单击**安装**。 **安装进度**窗格将显示在安装过程。 安装完成后，单击**关闭**。  
  

