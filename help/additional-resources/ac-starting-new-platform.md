---
title: 启动新平台
description: 详细了解在使用Adobe Campaign启动新平台时如何管理投放能力。
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

在新平台上开始发送电子邮件时，您应该非常小心，因为该平台没有任何使用历史，并且从未将发送IP用于此目的时没有信誉。

ISP自然会怀疑从未用于发送电子邮件、突然开始发送大量电子邮件流量的IP地址。 事实上，垃圾邮件发送者通常使用“未知”IP地址(从未被阻止列表的地址)在检测之前发送尽可能多的邮件。

在生产阶段刚开始时，您就不可能指望达到以产出衡量的操作速度。 此外，您不应尝试以这种速率发送消息，因为这可能导致ISP阻止发送地址并严重危害启动阶段的其余部分。

## 主要原则

下面列出了启动新平台时应遵循的主要原则。

* 配置专用于从Adobe发送电子邮件促销活动的专用子域。

* 如果你有这些信息， **将无效地址导入隔离表**.
首次使用地址列表且可能不完全合格时，经常会启动平台。 如果您发送到无效地址或蜜罐地址，这将有助于降低平台的声誉。

   * 如果您有一个无效地址列表，则最好在首次发送之前将其导入隔离表。 隔离表可通过以下方式获取： **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic)和 **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard)菜单。

   * 尽管如此，如果您希望重新鉴定无效地址，那么最好在平台信誉建立后逐个进行鉴定，以便“稀释”一段时间内对无效地址的使用。

* **限制吞吐率** 限制子域数量。 有关调整此类技术设置的更多信息，请与Adobe Campaign管理员联系。

* **逐步增加发送的数量** 以避免被标记为垃圾邮件。 不要从一开始就定位整个数据库，而是在每次发送时都添加一个额外的列表部分。 这样您就可以在每一步增加容量，同时降低无效地址的总速率。 为确保启动阶段的顺利进行，可以使用波段。

* **定期发送**. 从某种程度上讲，定期发送小照片比偶尔发送大广告要好。
* **请密切关注投放报告**. 高错误指示器可能意味着技术设置配置不正确。

## 其他资源

有关上述原则及其在Adobe Campaign中的实施的更多信息，请参阅以下部分：

* [利用 IP 预热提高您的电子邮件声誉](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [关于垃圾邮件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [通过隔离优化投放](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [确定整个平台的隔离地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多个批次发送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [投放监测](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html?lang=zh-Hans#sending-messages)

**Adobe Campaign Standard**

* [通过隔离优化投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [确定整个平台的隔离地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [监测投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html?lang=zh-Hans)
