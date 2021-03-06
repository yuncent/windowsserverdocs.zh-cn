---
title: 为租户 VM 网络适配器配置服务质量（QoS）
description: 为租户 VM 网络适配器配置 QoS 时，可以在数据中心桥接 \(DCB\)或软件定义的网络 \(SDN\) QoS 之间进行选择。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6d783ff6-7dd5-496c-9ed9-5c36612c6859
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: 61f790898d2a6068afb1436957e64861f4c0ebe8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309934"
---
# <a name="configure-quality-of-service-qos-for-a-tenant-vm-network-adapter"></a>为租户 VM 网络适配器配置服务质量（QoS）

>适用于：Windows Server（半年频道）、Windows Server 2016

为租户 VM 网络适配器配置 QoS 时，可以在数据中心桥接 \(DCB\)或软件定义的网络 \(SDN\) QoS 之间进行选择。

1.  **DCB**。 可以使用 Windows PowerShell NetQoS cmdlet 来配置 DCB。 有关示例，请参阅[远程直接内存访问（RDMA）和交换机嵌入式组合（SET）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)主题中的 "启用数据中心桥接" 部分。

2.  **SDN QoS**。 可以通过使用网络控制器启用 SDN QoS，此功能可设置为限制虚拟接口上的带宽，以防大流量 VM 阻止其他用户。  你还可以配置 SDN QoS，为 VM 保留特定的带宽量，以确保无论网络流量有多少，都可以访问 VM。  

通过网络接口属性的端口设置应用所有 SDN QoS 设置。 有关更多详细信息，请参阅下表。

|元素名称|说明|
|------------|-----------| 
|macSpoofing| 允许 Vm 将传出数据包中的源媒体访问控制 \(MAC\) 地址更改为未分配给 VM 的 MAC 地址。<p>允许的值：<ul><li>启用–使用其他 MAC 地址。</li><li>Disabled –只使用分配给它的 MAC 地址。</li></ul>|
|arpGuard| 允许 ARP 防护仅在 ArpFilter 中指定的地址通过端口。<p>允许的值：<ul><li>已启用-允许</li><li>禁用-不允许</li></ul>|
|dhcpGuard| 允许或删除虚拟机中声明为 DHCP 服务器的任何 DHCP 消息。 <p>允许的值：<ul><li>Enabled –丢弃 DHCP 消息，因为虚拟化的 DHCP 服务器被视为不受信任。</li><li>Disabled –允许收到消息，因为虚拟化的 DHCP 服务器被视为可信。</li></ul>|
|stormLimit| 每秒允许 VM 通过虚拟网络适配器发送的数据包数（广播、多播和未知单播）。 丢弃一秒时间间隔内超出限制的数据包。 值为零 \(0\) 表示没有限制。|
|portFlowLimit| 允许为端口执行的最大流数。 值为空或零 \(0\) 表示没有限制。 |
|vmqWeight| 相对权重描述虚拟网络适配器使用虚拟机队列（VMQ）的关联性。 值的范围是0到100。<p>允许的值：<ul><li>0–在虚拟网络适配器上禁用 VMQ。</li><li>1-100 –在虚拟网络适配器上启用 VMQ。</li></ul>|
|iovWeight| 相对权重设置虚拟网络适配器与分配的单根 i/o 虚拟化 \(SR-IOV\) 虚函数之间的关联。 <p>允许的值：<ul><li>0–在虚拟网络适配器上禁用 SR-IOV。</li><li>1-100 –在虚拟网络适配器上启用 SR-IOV。</li></ul>|
|iovInterruptModeration|<p>允许的值：<ul><li>默认值–物理网络适配器供应商的设置确定该值。</li><li>适应 </li><li>off </li><li>低</li><li>中</li><li>高</li></ul><p>如果选择 "**默认**值"，则物理网络适配器供应商的设置将确定该值。  如果选择 "**自适应**"，则运行时流量模式 determins 中断裁决速率。|
|iovQueuePairsRequested| 分配给 SR-IOV 虚拟函数的硬件队列对数。 如果需要 \(RSS\) 的接收方缩放，并且绑定到虚拟交换机的物理网络适配器支持 SR-IOV 虚函数上的 RSS，则需要多个队列对。 <p>允许的值：1到4294967295。|
|QosSettings| 配置以下 Qos 设置，所有这些设置都是可选的： <ul><li>**outboundReservedValue** -如果 outboundReservedMode 为 "绝对"，则值表示带宽（以 Mbits/s 为单位），可保证虚拟端口传输（传出）。 如果 outboundReservedMode 为 "权重"，则该值指示所保证的带宽的加权部分。</li><li>**outboundMaximumMbps** -表示虚拟端口（出口）允许的最大发送方带宽（以 Mbps 为单位）。</li><li>**InboundMaximumMbps** -表示虚拟端口（入口）允许的最大接收端带宽（以 Mbps 为单位）。</li></ul> |

---