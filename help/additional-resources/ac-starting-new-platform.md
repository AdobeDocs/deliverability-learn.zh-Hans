---
title: 启动新平台
description: 了解有关使用Adobe Campaign启动新平台时管理交付能力的更多信息。
feature: 付诸实践
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 3%

---


# 启动新平台 {#starting-new-platform}

在设置要与Adobe Campaign一起使用的新平台时，维护域和IP地址信誉至关重要。

## 敏感步骤

在开始在新平台上发送电子邮件时，您应当非常小心，因为该平台没有任何使用历史，而且当发送的IP从未用于此目的时，没有信誉。

ISP自然会怀疑从未用来发送电子邮件的IP地址，并突然开始发送大量电子邮件流量。 事实上，垃圾邮件发送者通常使用“未知”IP地址（从未过的地址）阻止列表在检测前发送尽可能多的邮件。

在生产阶段的开始，您不能期望在输出方面达到操作速度。 此外，您不应尝试以此速率发送消息，因为这可能会导致ISP阻止发送地址，并严重危害其余开始阶段。

## 主要原则

下面列出了启动新平台时应遵循的主要原则。

* 配置专用子域，该子域特定于从Adobe发送的电子邮件活动。

* 如果您有此信息，**将无效地址导入隔离表**。
首次使用地址列表时，启动平台通常会发生，但该地址可能未经完全限定。 如果发送到无效地址或蜜罐地址，这会降低平台的声誉。

   * 如果列表的地址无效，在首次发送之前将其导入隔离表符合您的最大利益。 隔离表可通过&#x200B;**[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]**(Campaign Classic)和&#x200B;**[!UICONTROL Administration > Channels > Quarantines > Addresses]**(Campaign Standard)菜单使用。

   * 但是，如果您仍希望重新指定无效地址，则最好在平台信誉建立后再逐位执行此操作，以便随着时间的推移“稀释”使用不良地址。

* **通过限制** mtachild数来限制吞吐量速率。有关调整此类技术设置的详细信息，请与Adobe Campaign管理员联系。

* **逐步增加发送的** 卷，以避免标记为垃圾邮件。不要从整个开始中目标整个列表库，而是每次发送时都额外添加一部分。 这样，您就可以在每一步增加音量，同时降低无效地址的总体速率。 为确保开始阶段的顺利开发，您可以使用批次。

* **定期发送**。从某种程度上讲，最好定期发送小照片，而不是偶尔发送大活动。
* **要密切关注投放报告**。高错误指示器可能意味着技术设置配置错误。

## Journey Orchestration

有关上述原则及其通过Adobe Campaign实现的更多信息，请参阅以下各节：

* [利用IP升温提高您的电子邮件声誉](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [关于垃圾邮件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [通过隔离优化投放](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [确定整个平台的隔离地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多个批次发送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [投放监控](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html#sending-messages)

**Adobe Campaign Standard**

* [通过隔离优化投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [确定整个平台的隔离地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [监控投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html)
