---
title: 数量 — 有关如何顺利过渡的提示
description: 您发送的邮件量对于建立良好的信誉至关重要。 了解如何才能顺利过渡。
topics: Deliverability
jira: KT-7055
thumbnail: kt7055.jpg
doc-type: article
activity: understand
role: Admin,User
level: Beginner
team: ACS
exl-id: 1bc56061-0c64-4033-b49c-66618916bca6
TQID: https://experienceleague.adobe.com/piIfp9yQkAa1F1bkO9zM7PcOConZhTrwARo4Y8wtMsQ
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2:
  - id: e64968b2-4ee5-47f9-8cae-0588f184b9eb
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 600
ht-degree: 1%

---

# 数量

您发送的邮件量对于建立良好的信誉至关重要。 设身处地为ISP着想 — 如果你开始看到来自陌生人的大量流量，那将会令人担忧。 立即发送大量邮件是有风险的，并且必然会导致通常难以解决的信誉问题。 将自己从声誉不佳、阻止因发送太快而造成的大量问题中解救出来，可能会令人沮丧、耗时且成本高昂。

数量阈值因ISP而异，也可能因平均参与量度而异。 有些发件人要求非常低且缓慢的体积斜坡，而其他发件人则可能要求更陡的体积斜坡。 我们建议与专家（如Adobe可交付性顾问）合作制定自定义的容量计划。

下面是有关如何顺利过渡的提示和提示列表：

* **权限**&#x200B;是任何成功的电子邮件计划的基础。
* **低和慢** — 从低发送量开始，然后随着您建立发件人信誉而增加。
* **串联邮件策略**&#x200B;允许您在不中断电子邮件日历的情况下增加Campaign的邮件量，同时使用当前的ESP结束邮件。
* **参与很重要** — 从定期打开并单击您电子邮件的订阅者开始。
* **遵循计划** — 我们的建议已帮助数百个Campaign客户成功增加了其电子邮件计划。
* **监视您的回复电子邮件帐户**。 对于您的客户来说，使用noreply@xyz.com或不做出响应是不好的体验。
* 非活动地址可能会对可投放性产生负面影响。 **重新激活并重新访问您当前的平台**，而不是新IP。
* **域** — 使用作为公司实际域的子域的发送域
   * 例如，如果您的公司域是xyz.com ，则email.xyz.com为ISP提供的信誉会比xyzemail.com更高
* **透明度** — 电子邮件域的注册详细信息应公开提供，不应为私人。

在许多情况下，事务性邮件不遵循传统的促销预热方法。 由于事务性邮件的性质，显然很难控制其数量，因为它通常需要用户交互来触发电子邮件触控。 在某些情况下，事务性邮件可以简单地在没有正式计划的情况下进行转换。 在其他情况下，随着时间推移而转换每种报文类型可能会有助于缓慢地增加报文量。 例如，您可能希望按如下方式进行过渡：

1. 购买确认 — 一般情况下参与度高
2. 购物车放弃 — 一般为中 — 高参与度
3. 欢迎电子邮件 — 高参与度，但可能包含错误地址，具体取决于您的列表收集方法
4. 回送电子邮件 — 通常参与度较低

## 产品特定资源

**Campaign**

* 在[本节](/help/additional-resources/ac-starting-new-platform.md)中了解有关使用Adobe Campaign启动新平台时管理可投放性的更多信息。
* 在[本节](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html?lang=zh-Hans#sending-using-multiple-waves)中了解如何通过Adobe Campaign Classic使用多个批次发送。
* 在[本节](/help/additional-resources/ac-domain-name-setup.md)中了解如何将子域完全委派给Adobe Campaign Classic或Standard。
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html?lang=zh-Hans) - *了解如何将子域完全委派给Adobe Campaign Classic。*
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html?lang=zh-Hans) - *了解如何将子域完全委派给Adobe Campaign Standard。*

## 其他资源

* 在[本节](/help/additional-resources/increase-reputation-with-ip-warming.md)中了解更多有关利用IP预热提高电子邮件信誉的信息。
