---
title: 启动新平台
description: 了解有关使用Adobe Campaign启动新平台时管理投放能力的更多信息。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 6c9ade01-3052-4311-af80-888294820024
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 8%

---

# 启动新平台 {#starting-new-platform}

在设置要与Adobe Campaign一起使用的新平台时，维护域和IP地址信誉至关重要。

## 敏感步骤

开始在新平台上发送电子邮件时，您应当非常谨慎，因为该平台没有任何使用历史，并且在发送IP从未用于此目的时没有声誉。

ISP自然会怀疑从未用来发送电子邮件的IP地址，而这些地址会突然开始发送大量电子邮件流量。 事实上，垃圾邮件发送者通常使用“未知”IP地址（从未过的地址）阻止列表来在检测前发送尽可能多的邮件。

在生产阶段开始时，您不能期望达到操作速度。 此外，您不应尝试以此速率发送消息，因为这可能会导致ISP阻止发送地址，并严重危害启动阶段的其余部分。

## 主要原则

下面列出了启动新平台时应遵循的主要原则。

* 配置专用子域，该子域专用于从Adobe发送的电子邮件促销活动。

* 如果你有这些信息， **将无效地址导入隔离表**.
首次使用地址列表（可能未完全限定）时，通常会启动平台。 如果您将发送到无效地址或蜜罐地址，这将有助于降低平台的声誉。

   * 如果您有无效地址列表，则在首次发送之前将其导入隔离表符合您的最大利益。 隔离表可通过 **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic)和 **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard)菜单。

   * 尽管如此，如果您希望重新指定无效地址，则最好在建立平台声誉后再逐位地执行此操作，以便随着时间的推移“稀释”不良地址的使用。

* **限制吞吐率** 限制母婴数量。 有关调整此类技术设置的更多信息，请与Adobe Campaign管理员联系。

* **逐步增加发送的卷** 以避免被标记为垃圾邮件。 请不要从一开始就定位整个数据库，而是在每次发送时添加列表的额外部分。 这样，您就可以在每一步增加卷，同时降低无效地址的总体速率。 为确保启动阶段的顺利开发，您可以使用批次。

* **定期发送**. 从某种程度上讲，最好定期发送小照片，而不是偶尔发送大型活动。
* **密切关注投放报告**. 高错误指示器可能意味着技术设置配置错误。

## 其他资源

有关上述原则及其在Adobe Campaign中实施的更多信息，请参阅以下各节：

* [利用 IP 预热提高您的电子邮件声誉](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [关于垃圾邮件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [通过隔离优化投放](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [确定整个平台的隔离地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多个批次发送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [投放监测](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html#sending-messages)

**Adobe Campaign Standard**

* [通过隔离优化投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [确定整个平台的隔离地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [监测投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html)
