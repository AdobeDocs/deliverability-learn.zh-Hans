---
title: 重新参与最佳实践
description: 了解如何通过重新参与战略提高交付能力。
feature: Journey Orchestration
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 1%

---


# 重新参与最佳实践 {#re-engagement}

在实施可交付性的同时，一些最佳做法包括尝试通过重新参与（或赢回）战略保持健康的用户群和提高可交付性。

* 保持健康的用户群是确保良好和一致的投放的主要方面之一。 许多可交付性问题源自于糟糕的数据实践和维护。
* 营销人员目前面临的最常见问题之一是不活跃的用户活动（也称为低参与或不参与），这会对电子邮件的投放和低ROI产生不利影响。

>[!NOTE]
>
>有关重新参与活动战略和Adobe的交付性服务的更多信息，请联系您的交付性顾问或与Adobe销售代理沟通。

## ISP如何视图非参与活动?{#how-do-isps-view-non-engagement-activity-}

多年来，ISP一直使用用户的参与反馈指标来决定放置消息的位置，或者是否应该发送消息。 用户[参与](/help/engagement.md)由正反馈和负反馈组成，ISP会持续监视这两者。 没有参与也许是消极参与的主要贡献者之一。 从可交付性角度看，一致地向没有参与的用户发送活动也会降低您的IP地址和域的整体信誉。

Gmail、Microsoft和OATH视图等ISP不参与恶意电子邮件和将邮件重定向到垃圾邮件文件夹的开始。 此外，这些订阅者可能不再拥有该电子邮件帐户，这可以用作“回收的”垃圾邮件陷阱。 这意味着该地址在一段时间内无效，所有消息都被拒绝。 如果您的订阅者管理系统未删除“硬退回”地址，则很可能会通过邮件发送到垃圾邮件陷阱，从而导致严重的投放问题。

## 你应该如何对待不活动？{#how-should-you-approach-inactivity-}

使用Adobe平台的客户可以通过查看打开的数据并根据细分点击数据来视图实例中的不活动。 由于不参与会妨碍投放，因此第一个想法是从数据库中删除用户。 然而，有时候，这可能被证明是错误的选择。 因此，重新参与（又称“回合”）策略是保留对接收邮件感兴趣的用户并逐步淘汰不再显示活动的用户的最佳建议。

## 重新参与活动真的有效吗？{#do-re-engagement-campaigns-really-work-}

根据“返回路径”研究，重新参与活动的结果是12%的开放率，而普通活动的平均开放率为14%。 尽管只有24%的订阅者阅读了重新参与活动，但约45%的订阅者阅读了后续消息。

![](../../help/assets/deliverability_implementation_1.png)

## 如何创建重新参与活动?{#how-do-you-create-a-re-engagement-campaign-}

### 阶段1 {#phase-1}

* 第一步是识别几乎没有打开或点击活动的用户，并相应地根据设置的时间范围对该组进行分段。 经验法则是审查在过去90天内未打开或点击电子邮件的订阅者。 但是，这会因业务性质（例如，季节性发送）而异。
* 定义时间框架时要记住的另一点是，ISP和阻止列表公司认为，参与时间介于1.5到1.8年之间。 此外，行为活动(如购买和网站活动)或其他接触点（如注册阶段或第一个联系点期间的首选项）。

### 阶段2 {#phase-2}

* 定义区段后，下一步是创建根据已识别的量度迎合订户的重新参与活动。 创建主题行有助于提高用户的兴趣。 根据Return Path的研究，声明“我们想念你”的主题行和内容生成的响应率高于“我们希望你回来”。
* 还可以奖励您重新使用电子邮件。 在考虑带折扣的优惠时，最好使用美元金额与百分比。 Return Path还建议这样做，因为它会产生更高的响应率。 最后，执行A/B拆分测试以查看响应和成功率也是一个有用的选项。

### 阶段3 {#phase-3}

下一步是确定重新参与活动的频率。 与重新确认消息不同，重新参与活动旨在随着时间的推移通过一系列电子邮件赢回用户。 以下示例提供了该频率的示例。

![](../../help/assets/deliverability_implementation_2.png)

按照打开或单击活动与活动交互的用户被添加回用户的参与列表。

### 阶段4 {#phase-4}

* 下一阶段是识别不断不显示活动的订阅者，并逐渐减少一段时间内向其发送电子邮件的次数。 如果过去一年中没有活动，则可以暂停订阅者电子邮件订阅。 尽管他们对电子邮件内容没有表现出任何兴趣，但始终有最后一次机会通过发送一次性重新确认订阅来重新激活其活动。
* 重新确认活动是询问长期处于非活动状态的订阅者是否希望保留在订阅列表的好方法。 创建活动时，最好添加“单击此处”链接，以便他们确认操作并验证其地址。 这样，操作就可以记录在数据库中。 以下是重新确认电子邮件的示例：

   ![](../../help/assets/deliverability_implementation_3.png)

   一旦用户采取了操作，就可以提供确认其重新订阅的登陆页。 以下是登陆页示例：

   ![](../../help/assets/deliverability_implementation_4.png)

## 产品特定资源

**Adobe Campaign**

* [跟踪日志Campaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [跟踪日志Campaign Standard](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Adobe客户历程管理**

* [消息跟踪](https://experienceleague.adobe.com/docs/customer-journey-management/using/reporting/message-tracking.html)