---
title: 数量 — 有关如何顺利过渡的提示
description: 您发送的邮件量对于建立良好的信誉至关重要。 了解如何才能顺利过渡。
topics: Deliverability
kt: 7055
thumbnail: kt7055.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 1bc56061-0c64-4033-b49c-66618916bca6
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 1%

---

# 数量

您发送的邮件量对于建立良好的信誉至关重要。 把你自己放在ISP的立场上 — 如果你开始看到不认识的人带来的大量流量，那将会令人担忧。 立即发送大量邮件是有风险的，并且必然会导致通常难以解决的信誉问题。 将自己从声誉不佳、阻止发送太快而造成的大量问题中解救出来，可能会令人沮丧、耗时且成本高昂。

数量阈值因ISP而异，也可能因平均参与量度而异。 一些发件人要求非常低且缓慢的音量斜坡，而其他发件人则可能允许更陡的音量斜坡。 我们建议与专家(如Adobe可交付性顾问)合作，制定定制的容量计划。

下面是有关如何顺利过渡的提示和提示列表：

* **权限** 是任何成功的电子邮件计划的基础。
* **低和慢**  — 从低发送量开始，然后随着您建立发件人信誉而增加。
* A **串联邮寄策略** 允许您在使用当前ESP逐渐减少的同时增加Campaign的音量，而不会中断电子邮件日历。
* **参与很重要**  — 从定期打开和单击您的电子邮件的订阅者开始。
* **遵循计划**  — 我们的建议已帮助数百个Campaign客户成功增加了其电子邮件项目。
* **监测回复电子邮件帐户**. 对于您的客户来说，使用noreply@xyz.com或拒绝响应是不好的体验。
* 非活动地址可能会对可投放性产生负面影响。 **在当前平台上重新激活并重新获得权限**，而不是您的新IP。
* **域**  — 使用作为公司实际域的子域的发送域
   * 例如，如果您的公司域是xyz.com ，则email.xyz.com为ISP提供了比xyzemail.com更高的可信度
* **透明度**  — 您的电子邮件域的注册详细信息应公开发布，而不应是私有的。

在许多情况下，事务性邮件并不遵循传统的促销暖身做法。 由于事务性邮件的性质，显然很难控制其数量，因为它通常需要用户交互来触发电子邮件触发。 在某些情况下，事务性邮件可以简单地在没有正式计划的情况下进行转换。 在其他情况下，随着时间推移转换每种报文类型可能更适合使流量缓慢增长。 例如，您可能希望按如下方式过渡：

1. 购买确认 — 一般而言，高参与度
2. 购物车放弃 — 中等 — 一般高参与度
3. 欢迎电子邮件 — 高参与度，但可能包含错误地址，具体取决于您的列表收集方法
4. 回送电子邮件 — 通常参与度较低

## 产品特定资源

**Campaign**

* 了解有关在中使用Adobe Campaign启动新平台时管理投放能力的更多信息 [本节](/help/additional-resources/ac-starting-new-platform.md).
* 了解如何在中通过Adobe Campaign Classic使用多个批次发送 [本节](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves).
* 了解如何在中将子域完全委派给Adobe Campaign Classic或Standard [本节](/help/additional-resources/ac-domain-name-setup.md).
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *了解如何将子域完全委派给Adobe Campaign Classic。*
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *了解如何将子域完全委派给Adobe Campaign Standard。*

## 其他资源

* 了解更多有关利用IP预热提高电子邮件声誉的信息，请参阅 [本节](/help/additional-resources/increase-reputation-with-ip-warming.md).
