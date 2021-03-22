---
title: 可交付性疑难解答
description: 了解如何确定和解决可交付性问题。
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
source-wordcount: '672'
ht-degree: 1%

---


# 可交付性疑难解答{#troubleshooting}

以下是一些最佳实践，可帮助确定和解决可交付性问题。

## 确定可交付性问题{#identify-deliverability-issue}

要确定可能的问题，该页面[上列出的元素可能会引起您的注意。](/help/ongoing-monitoring.md)

<!--
Mailing or campaign metrics: unsubscribe, abuse complaint and/or bounce rates are higher than usual.
Subscriber activity: opens, clicks and/or transactions are lower than usual.
Seed accounts show filtered or non-delivered mailings.
-->

## 假设潜在原因{#potential-causes}

请扪心自问以下问题，找出您的可交付性问题的可能原因：

* 列表细分最近是否有变化？
* 我是否获得了任何新数据源？
* 我是不是意外地寄了隔离文件？
* 问题是否可能是由于我的消息内容？
* 我是否足够频繁地发送邮件以保持温暖的IP?
* 我是按活动/参与还是发送完整文件来划分邮件？
* 从最近的情况看，我文件上的“安全”区段是什么？
* 对于未定义为安全的区段，我是否有重新激活和重新确认策略？

## 解决{#address-issue}问题

### 投诉

[投](/help/metrics/complaints.md) 诉由通过从收件箱中点击相应按钮将电子邮件报告为垃圾邮件的订阅者定义。

如果您的投放问题是由投诉引起的：
* 你必须努力确定收件人为什么抱怨。
* 您可能还希望考虑将取消订阅链接移至电子邮件顶部。 这将鼓励用户取消订阅，而不是抱怨垃圾邮件按钮。

发送方可以从其[反馈循环](/help/transition-process/infrastructure.md#feedback-loops)投诉中获取大量信息：
* 存储数据，并在选择加入来源、地址订阅时间、甚至某些行为人口统计等方面寻找模式，这一点很重要。
* 投诉通常可以识别文件中存在风险的数据源或细分。 “风险”被定义为最有可能抱怨，这会损害声誉，进而会损害收件箱费率。

抱怨还来自那些不再希望收到电子邮件的用户：
* 这通常是由于消息传递过度、用户对消息的感知、他们不期望消息，或者不记得选择加入。
* 还必须进行审计，以确保所有收集点都是清晰的，并确保您的收集点中没有预先选中的框。
* 您还应当在订阅者选择加入时发送一封欢迎电子邮件，以设置基调并说明他们预期收到您电子邮件的频率。

### 数据有效性

**当您发** 送给ISP的不可交付 **地址** 时，会出现硬跳转。地址可能因以下许多原因而无法传送：
* 地址拼写错误。 这可以通过实时数据验证服务解决，或在向该地址发送营销电子邮件之前要求确认的加入。
* 列表或数据源错误。 如果它来自新源，请查看地址的收集方式并确保获得许可。
* 邮寄到一次处于活动状态，但在一段时间不活动后已关闭或终止的地址。

### 参与度

除了投诉和数据有效性外，ISP们比以往任何时候都更关注&#x200B;**积极参与**，以作出投放决策。 他们希望了解您的订阅者是打开您的电子邮件，还是在不阅读的情况下删除它们。 由于他们不与发送方共享此数据，因此我们必须使用我们提供的信息，并将打开/点击/交易翻译为互动。

作为持续声誉维护的一部分，了解参与订阅者在您的列表上的情况并为每个文件上的订阅者制定&#x200B;**最近风险层次结构**&#x200B;非常重要。 最近期定义为上次打开/单击/处理或注册日期。 此时间帧可能因垂直而不同。 请按以下步骤执行此操作：

1. 确定每个垂直的活动（“安全”）段。 这通常是过去3-6个月内处于活动状态的订阅者。
1. 降低失活频率。
1. 创建[重新参与](/help/additional-resources/re-engagement.md)系列，以防出现中度风险失常。 这通常为6-9个月，无需参与。
1. 制定重新确认活动，以防发生更高的风险。 这通常是9-12个月内未使用电子邮件的订阅者。
1. 最后，设置放弃规则并删除在“x”个月内未打开您电子邮件的订阅者。 我们通常建议12个月以上，但这可能因销售和购买周期而异。