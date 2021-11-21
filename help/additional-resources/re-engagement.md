---
title: 重新接触最佳实践
description: 了解如何通过重新参与策略来提高投放能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 30118706-d4c0-4bd8-8c9b-50c26b8374ef
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '924'
ht-degree: 1%

---

# 重新接触最佳实践 {#re-engagement}

在实施投放能力时，一些最佳实践包括尝试通过重新参与（或双赢）策略来维护健康的订阅者群并提高投放能力。

* 维护健康的用户群是确保良好、一致投放的主要方面之一。 许多投放能力问题都源自于糟糕的数据实践和维护。
* 营销人员当前面临的最常见问题之一是用户活动不活跃（也称为低参与度或不参与度），这可能会对电子邮件的交付和低ROI产生不利影响。

>[!NOTE]
>
>有关重新参与促销活动策略和Adobe的可投放性服务的更多信息，请联系您的可投放性顾问，或与Adobe销售代理沟通。

## ISP如何查看非参与活动？ {#how-do-isps-view-non-engagement-activity-}

多年来，ISP一直使用用户的参与度反馈量度来决定在何处放置消息，或者是否应该发送消息。 用户 [参与](/help/engagement.md) 包括持续的正反馈和负反馈以及ISP监控器。 没有参与或许是消极参与的主要因素之一。 从投放能力的角度来看，持续向未显示参与度的用户发送促销活动，也会降低您IP地址和域的整体声誉。

Gmail、Microsoft®和OATH等ISP将不参与视为不需要的电子邮件，并开始将消息重定向到垃圾邮件文件夹。 此外，这些订阅者可能不再拥有电子邮件帐户，并且这可以用作“回收”垃圾邮件陷阱。 这意味着地址在一段时间内无效，所有消息都被拒绝。 如果您的订阅者管理系统未删除“硬退回”地址，则可能会通过邮件发送到垃圾邮件陷阱，从而导致重大投放问题。

## 您应该如何处理非活动状态？ {#how-should-you-approach-inactivity-}

使用Adobe平台的客户可以通过查看打开的数据并根据区段单击数据来查看其实例中的非活动状态。 由于不参与会妨碍交付，因此第一种想法是从数据库中删除订阅者。 然而，有时这可能被证明是错误的选择。 因此，重新参与（也称为“双赢”）策略是最好的建议，保留对接收邮件感兴趣的订阅者，并逐步淘汰不再显示活动的订阅者。

## 重新参与营销活动真的有效吗？ {#do-re-engagement-campaigns-really-work-}

根据回访路径研究，重新参与营销活动的结果是12%的打开率，而正常营销活动的平均打开率为14%。 尽管只有24%的订阅者阅读了重新参与活动，但约45%的订阅者阅读了后续消息。

![](../../help/assets/deliverability_implementation_1.png)

## 如何创建重新参与营销活动？ {#how-do-you-create-a-re-engagement-campaign-}

### 阶段1 {#phase-1}

* 第一步是识别很少甚至没有打开或单击活动的订阅者，并据此根据设置的时间范围对此组进行分段。 经验法则是审查过去90天内未打开或单击电子邮件的订阅者。 但是，这会因业务性质（例如，季节性发送）而异。
* 在定义时间范围时要记住的另一点是，ISP和阻止列表公司认为参与期介于1.5到1.8年之间。 此外，行为活动（如购买和网站活动）或其他接触点（如注册阶段或首次联系点期间的首选项）。

### 阶段2 {#phase-2}

* 定义客户群后，下一步是创建一个重新参与的营销活动，根据已识别的量度迎合订阅者。 创建主题线有助于提高用户的兴趣。 根据回访路径研究，声明“我们想念你”的主题行和内容产生的响应率比“我们希望你回来”高。
* 还可以为重新使用电子邮件提供激励。 在考虑具有折扣的选件时，最好使用美元金额与百分比。 回访路径还建议执行此操作，因为它会产生更高的响应率。 最后，执行A/B拆分测试以检查响应和成功率也是一个有用的选项。

### 阶段3 {#phase-3}

下一步是确定重新参与营销活动的频率。 与重新确认消息不同，重新参与营销活动旨在随着时间的推移通过一系列电子邮件为订阅者赢得回访。 以下示例提供了频度的示例。

![](../../help/assets/deliverability_implementation_2.png)

通过执行打开或点击活动参与营销活动的订阅者会添加到参与的订阅者列表中。

### 阶段4 {#phase-4}

* 下一阶段是确定持续不显示活动的订阅者，并逐步减少在一段时间内向他们发送电子邮件的次数。 如果过去一年没有活动，则最好暂停订阅者电子邮件订阅。 尽管他们对电子邮件内容不感兴趣，但始终有最后一次机会通过发送一次性重新确认营销活动来重新激活订阅。
* 重新确认营销活动是向长期处于不活跃状态的订阅者询问他们是否希望保留在订阅列表中的好方法。 创建营销活动时，最好添加“单击此处”链接，以便他们确认操作并验证其地址。 这样，操作便可以记录在数据库中。 以下是重新确认电子邮件的示例：

   ![](../../help/assets/deliverability_implementation_3.png)

   订阅者采取操作后，即可提供确认其重新订阅的登陆页面。 以下是登陆页面的示例：

   ![](../../help/assets/deliverability_implementation_4.png)

## 特定于产品的资源

**Adobe Campaign**

* [在Campaign Classic中跟踪日志](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/delivery-dashboard.html#tracking-logs)
* [在Campaign Standard中跟踪日志](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/sending-and-tracking-messages/tracking-messages.html#tracking-logs)

**Adobe客户历程管理**

* [消息跟踪](https://experienceleague.adobe.com/docs/journey-optimizer/using/reporting/message-tracking.html?lang=zh-Hans)
