---
title: 启动新平台
description: 详细了解在使用Adobe Campaign启动新平台时管理投放能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 6c9ade01-3052-4311-af80-888294820024
TQID: https://experienceleague.adobe.com/cQa5nOTSJwxDGX-QkXGez5dpm5N-8I7QZ-LsEW0FRLo
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2:
  - id: a075b2c1-7748-4328-b7f6-343aa314616a
  - id: c5474392-5419-4296-9e41-f6f4ce4f6e9b
  - id: c5f60233-d5ea-4453-a799-0ad258b4d399
  - id: d1d0a9cd-295d-4976-8c39-ddae266f240e
  - id: e2290edd-b061-4880-9d79-dee306cf5aa9
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
  - id: fdbb8fc9-ffa3-4b86-88fe-aa4c5a3e1bc6
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 666
ht-degree: 9%

---

# 启动新平台 {#starting-new-platform}

在设置要与Adobe Campaign一起使用的新平台时，维护域和IP地址信誉至关重要。

## 敏感步骤

在新平台上开始发送电子邮件时，您应该非常小心，因为该平台没有任何使用历史，并且从未将发送IP用于此目的时的信誉也不高。

ISP自然会怀疑从未用于发送电子邮件、突然开始发送大量电子邮件流量的IP地址。 事实上，垃圾邮件发送者通常使用“未知”IP地址（从未被阻止列表的地址）在检测之前发送尽可能多的邮件。

在生产阶段刚开始时，您就不可能指望达到以产出衡量的运行速度。 此外，您不应尝试以这种速率发送消息，因为这可能导致ISP阻塞发送地址并严重危害启动阶段的其余部分。

## 主要原则

下面列出了启动新平台时应遵循的主要原则。

* 配置一个专用子域，该子域特定于从Adobe发送的电子邮件营销活动。

* 如果您有此信息，请&#x200B;**将无效地址导入隔离表**。
首次使用地址列表且可能不完全合格时，经常会启动平台。 如果发送到无效地址或honeypot地址，这将有助于降低平台的声誉。

   * 如果您有一个无效地址列表，则在首次发送之前将其导入隔离表符合您的最大利益。 隔离表通过&#x200B;**[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic)和&#x200B;**[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard)菜单提供。

   * 同样，如果您希望重新限定无效地址，则最好在逐个建立平台的信誉之后这样做，以便“稀释”随着时间的推移使用无效地址。

* **通过限制mtachild的数量来限制吞吐率**。 有关调整此类技术设置的更多信息，请与Adobe Campaign管理员联系。

* **逐步增加发送的卷**&#x200B;以避免被标记为垃圾邮件。 不要从一开始就定位整个数据库，而是在每次发送时都添加列表的一个额外部分。 这样您就可以在每一步增加容量，同时降低无效地址的总速率。 为确保启动阶段的顺利发展，可以使用波段。

* **定期发送**。 从某种程度上讲，定期发送小批次比偶尔发送大批次要好。
* **请密切注意投放报告**。 高错误指示器可能意味着技术设置配置不正确。

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
