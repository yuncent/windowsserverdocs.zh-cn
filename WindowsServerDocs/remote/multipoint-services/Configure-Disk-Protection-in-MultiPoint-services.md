---
title: 在 MultiPoint 服务中配置磁盘保护
description: 了解如何设置 MultiPoint 服务的磁盘保护
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bd9bf5b9-e481-499b-9c15-7ee5a4f470c4
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: dc0a46b57753f08cc7d79fd05de7a9e81469cc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820168"
---
# <a name="configure-disk-protection"></a>配置磁盘保护
可以使用 Multipoint Services 中的磁盘保护以防止意外的更新，以计划 Windows 更新磁盘保护处于活动状态，但要临时禁用磁盘保护，并卸载磁盘保护时要保留您的系统卷。  
  
可以通过启用 MultiPoint Services 中的磁盘保护，保护系统卷 （驱动器 Windows 的安装位置，通常是 c:）从不需要的更改。 启用磁盘保护，对系统卷所做的更改存储在一个临时位置，以便只需重新启动计算机将其放弃并自动将使系统回到上一已知良好状态。  
  
管理员可以轻松地安装软件，或通过暂时禁用磁盘保护进行配置更改。 为了使系统保持最新的 Windows 更新和反恶意软件定义，磁盘保护计划一个维护时段来下载并安装更新。 管理员还可以提供自定义脚本，以适应 Windows 更新之外的任何维护需求维护时段内运行。  
  
## <a name="enable-disk-protection"></a>启用磁盘保护  
启用磁盘保护之前，请确保所有应用程序和驱动程序已安装并保持最新的并将用户配置文件移动到不受保护的卷。 如果你需要进行手动更新后启用磁盘保护，可以暂时禁用磁盘保护。 但是，它是最简单的方法之前启用磁盘保护，系统会进入理想状态。  
  
 
1.  登录到以管理员身份运行 MultiPoint 服务的服务器上。  
  
2.  之前启用磁盘保护：  
  
    -   请确保 MultiPoint 服务系统处于完全想它保持的状态。 例如，确保已安装的软件、 系统设置和更新正确。  
  
    -   将用户配置文件移动到未受保护，或设置由系统卷共享的文件位置，如中所述的卷[启用文件共享在 MultiPoint Services 中](Enable-file-sharing-in-MultiPoint-services.md)。  
  
3.  从**启动**屏幕上，打开**MultiPoint 管理器**。  
  
4.  单击**主页**选项卡上，单击**启用磁盘保护**，然后单击**确定**。  
  
第一次启用磁盘保护时，系统准备通过安装驱动程序并创建系统卷上的缓存文件。 缓存文件将临时存储磁盘保护处于活动状态时对系统卷所做的任何更改。 系统更新存储在缓存文件，因为它们不会更改的缓存文件的外部卷的受保护的内容。 每次系统启动时，缓存文件重置，则该命令可放弃自上一系统启动以来那里存储的任何更改。 因此，在系统始终启动与启用磁盘保护的相同的状态。  
  
Windows 需要更新一些系统文件 – 其中包括系统页面文件、 故障转储位置和事件日志。 启用磁盘保护时，不会被丢弃这些文件。 若要实现此目的，首次启用磁盘保护，并且将这些文件移到该卷，则创建名为 DpReserved 新卷。 DpReserved 分区未受保护，因此对这些文件的写操作通过重新启动时，即使在磁盘保护启用了持久保留。  
  
## <a name="schedule-software-updates"></a>定时计划软件更新  
如果 Windows 配置为自动安装 Windows 更新，磁盘保护允许这些更新的配置的时间，并且不会放弃更新。 例如，如果 Windows 更新计划为凌晨 3:00，磁盘保护检查更新每一天的凌晨 3:00 如果找到任何更新，MultiPoint 服务暂时禁用磁盘保护、 更新，应用，然后重新启用磁盘保护。  
   
1.  在 MultiPoint 管理器中显示**主页**选项卡，然后依次**定时计划软件更新**。  
  
2.  在计划软件更新对话框中，单击**在更新**，并选择更新的时间，例如，**上午 3:00**。  
  
3.  选择**运行 Windows 更新**复选框。  
  
4.  如果你的组织运行其自身的更新脚本，选择**运行以下程序**复选框，并指定你组织的更新脚本的位置。  
  
5.  选择要允许更新运行的最大时间。  
  
6.  下**完成后**，选择是否让系统返回到其以前的电源状态或关闭应用更新后的情况下。  
  
7.  单击 **“确定”**。  
  
## <a name="temporarily-disable-disk-protection"></a>暂时禁用磁盘保护  
如果管理员需要安装软件、 更改系统设置或执行其他维护任务涉及系统更新的它们可以暂时禁用磁盘保护。 进行更改后，重新启用磁盘保护。 在系统重新启动时，系统将在实际应用时已启用磁盘保护保留其状态。  
    
1.  在 MultiPoint 管理器中，单击**主页**选项卡。  
  
2.  在主页选项卡上单击**禁用磁盘保护**，然后单击**确定**。  
  
> [!NOTE]  
> 请记住在维护完成后重新启用磁盘保护。 直到管理员显式重新启用磁盘保护，将不会再次保护系统。  
  
## <a name="uninstall-disk-protection"></a>卸载磁盘保护  
卸载磁盘保护中删除该驱动程序和缓存文件，以便你才应该这样做如果你想要停止使用磁盘保护长期。 如果只是想要执行维护或暂时停止保护，请改为使用禁用磁盘保护任务。  
  
你可以卸载磁盘保护，它是启用还是禁用。  
   
1.  在 MultiPoint 管理器中，单击**主页**选项卡。  
  
2.  主页选项卡上，然后单击**卸载磁盘保护**，然后单击**确定**。  
  
    单击后**确定**，在计算机重新启动。 卸载过程需要多个重启，在此期间删除的驱动程序和缓存文件。 DpReserved 分区会保留，和的页面文件、 故障转储位置和事件日志文件仍配置为使用 DpReserved 分区。