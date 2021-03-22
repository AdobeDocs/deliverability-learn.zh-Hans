---
title: 跳出次数
description: 了解不同类型的弹回。
feature: 量度
topics: Deliverability
kt: 7047
thumbnail: kt7047.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 283f1cb2bb40818e11daa1a3753e8428b47e08ee
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 4%

---


# 跳出次数

弹回是投放尝试和失败的结果，ISP会提供返回故障通知。 弹回处理是列表卫生的关键部分。 给定电子邮件已连续多次弹回后，此过程会标记其以抑制。 触发抑制所需的跳出次数和类型因系统而异。 此过程会阻止系统继续发送无效的电子邮件地址。 弹回是ISP用来确定IP信誉的关键数据之一。 关注此指标非常重要。 “已送达”与“已退回”可能是衡量营销信息投放的最常见方式：交付率越高越好。

我们会挖出两种不同的弹跳。

## 硬弹回

硬弹回是在ISP确定对用户地址的邮寄尝试为不可交付后生成的永久故障。 在Adobe Campaign中，分类为不可交付的硬弹回将添加到隔离，这意味着不会重新尝试它们。 在某些情况下，如果故障原因不明，硬跳出会被忽略。
以下是一些硬弹回的常见示例：

* 地址不存在
* 帐户已禁用
* 语法错误
* 域错误

## 软弹回

软跳回是ISP在发送邮件时遇到的临时故障。 为了尝试成功投放，软失败将重试多次(出现差异，具体取决于使用自定义或现成投放设置)。 在尝试最大数量的重试之前，不会向隔离添加持续软跳出的地址（这同样会因设置而异）。 软弹回的一些常见原因包括：

* 邮箱已满
* 向下接收电子邮件服务器
* 发件人声誉问题

![跳出类型](../assets/bounce-types.png)

>[!NOTE]
>
>回报是声誉问题的关键指标，因为它们可以突出坏数据源（硬回报）或ISP的声誉问题（软回报）。
>
>软弹回通常作为发送电子邮件的一部分发生，在重试处理期间应允许解决软弹回问题，然后将其描述为真正的可交付性问题。 如果您的软反弹率超过单个ISP的30%且未在24小时内解决，则最好向您的Adobe Campaign交付能力顾问提出疑虑。

## 产品特定资源

**Adobe Campaign Classic**

* [投放失败类型和原因](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#delivery-failure-types-and-reasons)
* [弹回邮件管理](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#bounce-mail-management)
* [非交付件和退回报告](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/global-reports.html#non-deliverables-and-bounces)

**Adobe Campaign Standard**

* [投放失败类型和原因](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html#delivery-failure-types-and-reasons)
* [退回邮件鉴别](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-delivery-failures.html#bounce-mail-qualification)
* [跳出摘要报表](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/bounce-summary.html?lang=en#reporting)
