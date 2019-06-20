---
title: 用户帐户控制的工作原理
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 7f5ad234959ebfe0687b8bc10f9e43f2092252f2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823548"
---
# <a name="how-user-account-control-works"></a>用户帐户控制的工作原理

>适用于：Windows 服务器 （半年频道），Windows Server 2016

用户帐户控制 (UAC) 有助于防止恶意程序（也称为恶意软件）损坏计算机，并帮助组织部署更好的托管桌面。 通过 UAC，应用程序和任务可始终在非管理员帐户的安全上下文中运行，除非管理员特别授予管理员级别的系统访问权限。 UAC 可以阻止未经授权的应用程序自动进行安装，并防止无意中更改系统设置。

## <a name="uac-process-and-interactions"></a>UAC 进程和交互
每个需要管理员访问令牌的应用程序必须提示管理员给予同意。 一种例外情况是父进程和子进程之间存在关系。 子进程会从父进程继承用户访问令牌。 但是，父进程和子进程必须具有相同的完整性级别。 Windows Server 2012 通过标记其完整性级别来保护的进程。 完整性级别是对信任的衡量。 “高”完整性应用程序执行修改系统数据的任务（如，磁盘分区应用程序），而“低”完整性应用程序则执行可能潜在损坏操作系统的任务（如，Web 浏览器）。 具有低完整性级别的应用程序无法修改具有高完整性级别的应用程序中的数据。 当标准用户尝试运行需要管理员访问令牌的应用程序时，UAC 会要求该用户提供有效的管理员凭据。

为了更好地了解此过程如何发生务必查看 Windows Server 2012 登录过程的详细信息。

### <a name="windows-server-2012-logon-process"></a>Windows Server 2012 Logon Process
下图演示了管理员的登录进程与标准用户的登录进程有何差异。

![演示如何为管理员身份登录过程不同于标准用户在登录过程的图](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

默认情况下，标准用户和管理员都会在标准用户的安全上下文中访问资源和运行应用程序。 当用户登录到计算机时，系统会为该用户创建一个访问令牌。 访问令牌包含有关向用户授予的访问级别的信息，包括特定安全标识符 (SID) 和 Windows 权限。

当管理员登录时，系统会为该用户创建两个单独的访问令牌：一个标准用户访问令牌和一个管理员访问令牌。 标准用户访问令牌包含与管理员访问令牌相同的用户特定信息，只是删除了 Windows 管理权限和 SID。 标准用户访问令牌用来启动不执行管理任务的应用程序（标准用户应用程序）。 之后，标准用户访问令牌将用来显示桌面 (Explorer.exe)。 Explorer.exe 是父进程，用户启动的其他所有进程都将从该进程继承访问令牌。 因此，所有应用程序都将以标准用户身份运行，除非用户给予同意或提供凭据以批准应用程序使用完全管理访问令牌。

是管理员组的成员的用户可以登录、 浏览 Web，并使用标准用户访问令牌时，读取电子邮件。 当管理员需要对其执行自动需要管理员访问令牌，Windows Server 2012 的任务会提示用户批准。 此提示称为提升提示，其行为可以通过使用本地安全策略管理单元 (Secpol.msc) 或组策略进行配置。

> [!NOTE]
> 使用术语"提升"来指代 Windows Server 2012 的提示用户同意或提供凭据以使用完全管理员访问令牌中的过程。

### <a name="the-uac-user-experience"></a>UAC 用户体验
启用 UAC 后，标准用户的用户体验将不同于管理员批准模式中管理员的用户体验。 运行 Windows Server 2012 的建议的更安全的方法是使您的主要用户帐户的标准用户帐户。 以标准用户身份运行将有助于最大程度地确保托管环境的安全性。 使用内置的 UAC 提升组件，标准用户可以通过输入本地管理员帐户的有效凭据来轻松执行管理任务。 用于标准用户的默认内置 UAC 提升组件就是凭据提示。

以标准用户身份运行的替代方法是在管理员批准模式中以管理员身份运行。 使用内置的 UAC 提升组件，本地管理员组的成员可以轻松地通过提供批准执行管理任务。 用于管理员批准模式中管理员帐户的默认内置 UAC 提升组件称为同意提示。 UAC 提升提示行为可以通过使用本地安全策略管理单元 (Secpol.msc) 或组策略进行配置。

**同意提示和凭据提示**

启用了 UAC，Windows Server 2012 提示同意或提供有效的本地管理员帐户的凭据启动的程序或需要完全管理员访问令牌的任务之前。 此提示可确保不会在无提示的情况下安装恶意软件。

**同意提示**

当用户尝试执行需要用户管理访问令牌的任务时，系统会出现同意提示。 下面是 UAC 同意提示的屏幕截图。

![UAC 同意提示的屏幕截图](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**凭据提示**

当标准用户尝试执行需要用户管理访问令牌的任务时，系统会出现凭据提示。 此标准用户默认提示行为可以通过使用本地安全策略管理单元 (Secpol.msc) 或组策略进行配置。 此外可以要求管理员通过设置用户帐户控制提供其凭据：凭据将值设置为提示管理员批准模式策略中的管理员的提升提示行为。

以下屏幕截图是 UAC 凭据提示的示例。

![显示 UAC 凭据提示的示例的屏幕截图](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**UAC 提升提示**

UAC 提升提示针对应用程序进行了颜色编码，从而可以立即识别应用程序的潜在安全风险。 当应用程序尝试使用管理员的完全存取令牌运行时，Windows Server 2012 将首先分析以确定其发布服务器的可执行文件。 应用程序首先被划分为基于可执行文件的发布服务器上的三个类别：Windows Server 2012 中，发布服务器验证 （有符号） 和未验证的发布服务器 （无符号）。 下图说明了 Windows Server 2012 如何确定向用户显示的颜色的提升提示。

提升提示的颜色编码如下所示：

-   红色背景带红色防火墙图标：应用程序被组策略阻止，或从被阻止的发布者。

-   蓝色背景带蓝黄相间图标：应用程序是管理的 Windows Server 2012 的应用程序中使用，例如控制面板项。

-   蓝色背景带蓝色防火墙图标：应用程序通过使用验证码签名，并且受本地计算机。

-   黄色背景带黄色防火墙图标：应用程序未签名或签名，但尚不受本地计算机。

**防火墙图标**

某些控制面板项（如“日期和时间”属性）同时包含管理员操作和标准用户操作。 标准用户可以查看时钟和更改时区，但是若要更改本地系统时间，则需要完全管理员访问令牌。 以下是“日期和时间”属性控制面板项的屏幕截图。

![屏幕截图显示 * * 日期和时间属性 * * 控制面板项](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

“更改日期和时间”按钮上的防火墙图标指示该进程需要完全管理员访问令牌，并将显示 UAC 提升提示。

**确保提升提示的安全**

通过将提示定向到安全桌面，可以进一步确保提升进程的安全。 Windows Server 2012 中默认情况下在安全桌面上显示同意提示和凭据提示。 只有 Windows 进程可以访问安全桌面。 对于更高级别的安全性，我们建议保持**用户帐户控制：提示提升时切换到安全桌面**启用策略设置。

当可执行文件请求提升时，交互式桌面（也称为用户桌面）便会切换到安全桌面。 安全桌面会隐去用户桌面，并显示必须响应才能继续的提升提示。 当用户单击是或不可以，桌面会切换回用户桌面。

恶意软件可以显示模拟的安全桌面，但用户帐户控制：同意策略设置设为提示管理员批准模式中管理员的提升提示行为，恶意软件不会获得提升如果用户单击是桌面上。 如果策略设置设置为提示输入凭据，模拟凭据提示的恶意软件可能能够从用户收集的凭据。 但是，恶意软件不会获得提升的权限，而且系统还具有其他防护措施，可以避免恶意软件控制用户界面，即使这些软件具有截获的密码。

虽然恶意软件可以显示模拟的安全桌面，但是此问题不会发生，除非用户以前在计算机上安装了恶意软件。 由于在启用 UAC 的情况下，需要管理员访问令牌的进程无法进行无提示安装，因此用户必须通过单击“是”或提供管理员凭据来明确给予同意。 UAC 提升提示的具体行为取决于组策略。

## <a name="uac-architecture"></a>UAC 体系结构
下图详细介绍了 UAC 体系结构。

![详细介绍了 UAC 体系结构关系图](../media/How-User-Account-Control-Works/UACArchitecture.gif)

若要更好地了解每个组件，请查看下表：

|组件|描述|
|-------|--------|
|**用户**||
|用户执行需要权限的操作|如果操作更改文件系统或注册表，则会调用虚拟化。 所有其他操作均调用 ShellExecute。|
|ShellExecute|ShellExecute 调用 CreateProcess。 ShellExecute 会从 CreateProcess 中查找 ERROR_ELEVATION_REQUIRED 错误。 如果接收到错误，ShellExecute 会调用应用程序信息服务以尝试通过提升提示执行请求的任务。|
|CreateProcess|如果应用程序要求提升，则 CreateProcess 会拒绝出现 ERROR_ELEVATION_REQUIRED 的调用。|
|**System**||
|应用程序信息服务|一种系统服务，可帮助启动运行时需要一种或多种提升权限的应用程序（如，本地管理任务），以及需要更高完整性级别的应用程序。 应用程序信息服务通过以下方式帮助启动此类应用程序：在要求提升且用户同意（取决于组策略）执行此操作时，使用管理用户的完全访问令牌为应用程序创建一个新的进程。|
|提升 ActiveX 安装|如果未安装 ActiveX，则系统会检查 UAC 滑块级别。 如果安装 ActiveX，**用户帐户控制：提示提升时切换到安全桌面**选中组策略设置。|
|检查 UAC 滑块级别|UAC 现在有四个通知级别可供选择，而且还有一个用于选择通知级别的滑块：<br /><br /><ul><li>高<br /><br />    如果将滑块设置为“始终通知”，则系统会检查安全桌面是否启用。</li><li>中等<br /><br />    如果将滑块设置为**默认通知仅当程序尝试更改我的计算机时我**，则**用户帐户控制：只提升签名并验证的可执行文件**选中策略设置：<br /><br /><ul><li>如果启用了该策略设置，则将对给定的可执行文件强制执行公钥基础结构 (PKI) 证书路径验证，然后才允许运行该文件。</li><li>如果未启用该策略设置（默认），则在允许运行给定的可执行文件之前，不会强制执行 PKI 证书路径验证。 **用户帐户控制：提示提升时切换到安全桌面**选中组策略设置。</li></ul></li><li>低<br /><br />    如果将滑块设置为“仅当程序尝试更改计算机时通知我(不降低桌面亮度)”，则会调用 CreateProcess。</li><li>从不通知<br /><br />    如果将滑块设置为**永远不通知我时**，UAC 提示将永远不会在程序尝试安装或尝试在计算机上进行任何更改时通知。 **重要说明：**   不建议使用此设置。 此设置是设置相同**用户帐户控制：管理员批准模式中管理员的提升提示行为**策略设置可**而不提示提升**。</li></ul>|
|是否启用安全桌面|**用户帐户控制：提示提升时切换到安全桌面**选中策略设置：<br /><br />-如果启用安全桌面，则所有提升请求都会都转到安全桌面而不考虑管理员和标准用户的提示行为策略设置。<br />-如果未启用安全桌面，则所有提升请求都会都转到交互式用户桌面，并使用管理员和标准用户的每个用户设置。|
|CreateProcess|CreateProcess 会调用 AppCompat、融合和安装程序检测以评估应用程序是否需要提升。 然后，将会检查可执行文件以确定其请求的执行级别，该级别存储在可执行文件的应用程序清单中。 如果清单中指定的请求执行级别与访问令牌不匹配，则 CreateProcess 将会失败，并向 ShellExecute 返回错误 (ERROR_ELEVATION_REQUIRED)。 |
|AppCompat|AppCompat 数据库存储应用程序的兼容性修补程序项中的信息。|
|融合|融合数据库存储应用程序描述清单中的信息。 清单架构将会更新以添加新的请求执行级别字段。|
|安装程序检测|安装程序检测可检测安装程序可执行文件，从而帮助防止在用户不知情和未经用户同意的情况下运行安装。|
|**内核**||
|虚拟化|虚拟化技术可确保不兼容的应用程序不会在运行失败时或无法确定失败原因时不出现提示。 UAC 还为写入到受保护区域的应用程序提供文件和注册表虚拟化以及日志记录。|
|文件系统和注册表|每用户文件和注册表虚拟化会将每计算机注册表和文件写入请求重定向到对等的每用户位置。 读取请求会首先重定向到虚拟化的每用户位置，然后再重定向到每计算机位置。|

没有从以前的 Windows 版本上 Windows Server 2012 UAC 的更改。 新的滑块将永远不会关闭 UAC 完全。 将新设置：

-   保持运行的 UAC 服务。

-   原因启动管理员而不显示 UAC 提示是自动批准所有提升都请求。

-   自动拒绝为标准用户的所有提升请求。

> [!IMPORTANT]
> 若要完全禁用 UAC，则必须禁用策略**用户帐户控制：以管理员批准模式运行所有管理员**。

> [!WARNING]
> 定制应用程序将无法在 Windows Server 2012 时禁用 UAC。

### <a name="virtualization"></a>虚拟化
由于企业环境中的系统管理员力求确保系统的安全，因此设计的许多业务线 (LOB) 应用程序都仅使用标准用户访问令牌。 因此，IT 管理员不需要时启用了 UAC 的运行 Windows Server 2012 替换大多数应用程序。

 Windows Server 2012 包含不兼容 UAC 且需要管理员的访问令牌才能正常运行的应用程序的文件和注册表虚拟化技术。 虚拟化可确保即使不兼容 UAC 的应用程序与 Windows Server 2012 兼容。 当不兼容 UAC 的管理应用程序尝试写入受保护的目录（例如 Program Files）时，UAC 将为该应用程序提供一个其自己尝试更改的资源的虚拟化视图。 该虚拟化副本存放在用户的配置文件中。 此策略将为每个运行非兼容应用程序的用户创建一个单独的虚拟化文件副本。

大多数应用程序任务都可使用虚拟化功能正常运行。 尽管虚拟化允许大多数应用程序运行，但这只是短期的修补程序，而不是长期的解决方案。 应用程序开发人员应修改其应用程序能够符合 Windows Server 2012 徽标计划越早越好，而不是依赖于文件、 文件夹和注册表虚拟化。

对于下列情形，不提供虚拟化选项：

1.  虚拟化不适用于使用完全管理访问令牌提升并运行的应用程序。

2.  虚拟化仅支持 32 位应用程序。 当未提升的 64 位应用程序尝试获取 Windows 对象的句柄（唯一标识符）时，它们只会收到访问拒绝消息。 本机 Windows 64 位应用程序需要与 UAC 兼容并且可将数据写入正确的位置。

3.  如果某个应用程序的清单含有请求的执行级别属性，则会为该应用程序禁用虚拟化。

### <a name="request-execution-levels"></a>请求执行级别
应用程序清单是一个 XML 文件，该文件描述并标识应用程序在运行时应绑定到的共享和专用并排程序集。 在 Windows Server 2012 中，应用程序清单包含用于实现 UAC 应用程序兼容性的条目。 管理应用程序在应用程序清单中包含一个条目，可提示用户提供对用户访问令牌的访问许可。 虽然这些应用程序在应用程序清单中缺少一个条目，但是大多数管理应用程序都可以通过使用应用程序兼容性修补程序，无需修改即可运行。 应用程序兼容性修补程序是数据库条目，使不兼容 UAC 正常运行 Windows Server 2012 的应用程序。

所有兼容 UAC 的应用程序应在应用程序清单中添加了一个请求的执行级别。 如果应用程序需要系统的管理访问权限，则用“需要管理员”的请求执行级别标记应用程序，可以确保系统将此程序标识为管理应用程序，并执行必需的提升步骤。 请求的执行级别用于指定应用程序所需的权限。

### <a name="installer-detection-technology"></a>安装程序检测技术
安装程序是用于部署软件的应用程序。 大多数安装程序会写入到系统目录和注册表项中。 在安装程序检测技术中，这些受保护的系统位置通常只可由管理员写入，这意味着标准用户不具有安装程序的足够访问权限。 Windows Server 2012 会试探性地检测安装程序和请求提供管理员凭据或来自管理员用户的批准才能运行具有访问权限。 Windows Server 2012 还会试探性地检测更新和卸载应用程序的程序。 UAC 的其中一个设计目标是防止在用户不知情和未经用户同意的情况下进行安装，因为安装程序会写入到系统文件和注册表的受保护区域。

安装程序检测仅适用于：

-   32 位可执行文件。

-   不具有请求的执行级别属性的应用程序。

-   在启用 UAC 的情况下以标准用户身份运行的交互式进程。

在创建 32 位进程之前，将会检查下列属性以确定其是否为安装程序：

-   文件名是否包含“install”、“setup”或“update”等关键字。

-   版本控制资源字段包含以下关键字：供应商、 公司名称、 产品名称、 文件描述、 原始文件名、 内部名称和导出名称。

-   并排清单中的关键字是否嵌入在可执行文件中。

-   特定 StringTable 条目中的关键字是否链接在可执行文件中。

-   资源脚本数据中的关键属性是否链接在可执行文件中。

-   可执行文件中是否存在字节的目标序列。

> [!NOTE]
> 关键字和字节序列是从利用多种安装程序技术观察出的共同特征派生而来的。

> [!NOTE]
> 用户帐户控制：检测应用程序安装并提示提升策略设置必须启用安装程序检测才能检测安装程序。 此设置在默认情况下处于启用状态，并且可以通过使用本地安全策略管理单元 (Secpol.msc) 进行本地配置，或通过组策略 (Gpedit.msc) 针对域、OU 或特定组进行配置。

