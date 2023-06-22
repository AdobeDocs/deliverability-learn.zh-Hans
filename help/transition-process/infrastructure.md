---
title: 基础结构
description: 了解需要什么才能正确地构造电子邮件基础架构。
topics: Deliverability
jira: KT-7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 2%

---

# 基础结构

成功的可交付性取决于坚实的基础。 电子邮件基础架构是一个核心元素。 正确构建的电子邮件基础架构包括多个组件，即域和IP地址。 这些组件就像您发送的电子邮件背后的机器，通常是发送信誉的锚点。 可投放性顾问会确保在实施期间正确设置这些元素，但由于信誉元素，因此您务必要了解此基本信息。

## 域设置和策略 {#domain-setup-and-strategy}

时代已经改变，一些ISP（如Gmail和Yahoo）现在将域信誉作为附加点向发件人附加电子邮件信誉。 您的域信誉基于您的发送域，而不是IP地址。 这意味着当做出ISP过滤决策时，您的品牌优先。

Adobe平台上新发件人入门流程的一部分包括设置发送域并确保正确建立您的基础架构。 您应与专家合作，了解您计划长期使用哪些域。 以下是一些可形成良好域策略的提示：

* 对于您选择的域，请尽可能清晰地反映品牌，这样用户就不会将邮件错误地识别为垃圾邮件。 例如newsletter.foo.com、receipts.foo.com等。
* 您不应使用父域或公司域，因为这样可能会影响从您的组织向ISP的邮件投放。
* 考虑使用父域的子域来使发送域合法化。
* 为事务型消息和营销型消息类别划分子域。 这将有助于您的电子邮件流量在ISP寻找此发送方法时更加可靠，这是已知的电子邮件最佳实践，强烈推荐。

## IP策略 {#ip-strategy}

形成结构良好的IP策略以帮助建立良好的信誉是非常重要的。 IP的数量和设置因您的业务模式和营销目标而异。 与专家合作，制定明确的战略以从头开始。 请注意以下重要事项：

* **IP过多** 会触发信誉问题，因为这是垃圾邮件发送者的常见策略， **snowshoe**，这是垃圾邮件发送者使用的一种策略，其通信量分布在多个IP上，以最大限度地提高垃圾邮件投放。 即使您不是垃圾邮件发送者，但如果您使用的IP过多，尤其是这些IP之前没有任何流量时，您可能会看起来像一个。
* **IP太少** 可能导致吞吐量问题并可能触发信誉问题。 吞吐量因ISP而异。 ISP愿意接受的程度和速度通常取决于其基础架构和发送信誉阈值。
* 为消息传送类型划分流量是关键。 在单独的IP池上，至少将营销和事务性邮件分隔开，这一点很重要。
* 根据您的邮件策略，如果您的信誉大为不同，还建议在不同的IP池上分离不同的产品或营销流。 一些营销人员还按地区进行细分。 将IP地址隔离给信誉较低的流量，并不能解决信誉问题，但可以防止“良好”信誉的电子邮件投放出现问题。 毕竟，你不想为了风险更高而牺牲好听众。

## 反馈循环 {#feedback-loops}

在幕后，Adobe平台正在处理有关退回、投诉、取消订阅等的数据。 这些反馈回路的设置是可投放性的一个重要方面。 投诉可能会损害声誉，因此您应该使用电子邮件地址来登记目标受众的投诉。 请务必注意，Gmail不会将这些数据提供回来。 列出取消订阅标头和参与筛选对Gmail订阅者来说尤其重要，他们现在构成了订阅者数据库的大多数。

## 身份验证 {#authentication}

身份验证是ISP用于验证发件人身份的过程。 两种最常见的身份验证协议是 [!DNL Sender Policy Framework] (SPF)和 [!DNL DomainKeys Identified Mail] (DKIM)。 最终用户看不到这些内容，但可以帮助ISP过滤来自已验证发件人的电子邮件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)越来越受欢迎，尽管其政策尚未被所有ISP纳入其声誉体系。

### SPF

[!DNL Sender Policy Framework] (SPF)是一种身份验证方法，它允许域的所有者指定他们使用哪些邮件服务器从该域发送邮件。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一种用于检测伪造发件人地址（通常称为欺骗）的身份验证方法。 如果启用了DKIM，则允许接收者确认是否授权发件人从该域发送邮件。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一种身份验证方法，允许域所有者保护其域免受未经授权的使用。 DMARC使用SPF或DKIM或同时使用两者来允许域所有者控制身份验证失败的邮件发生的情况：已投放、隔离或拒绝。

## 产品特定资源

**Campaign**

* 了解如何在中将子域完全委派给Adobe Campaign Classic或Standard [本节](/help/additional-resources/ac-domain-name-setup.md).
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *了解如何将子域完全委派给Adobe Campaign Classic。*
* [控制面板：完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html) - *了解如何将子域完全委派给Adobe Campaign Standard。*
* 了解有关在中为Campaign Classic实例实施反馈循环的更多信息 [本节](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc).

## 其他资源

* 在中了解有关SPF、DKIM和DMARC身份验证方法的更多信息 [本节](/help/additional-resources/authentication.md).
* 了解更多有关利用IP预热提高电子邮件声誉的信息，请参阅 [本节](/help/additional-resources/increase-reputation-with-ip-warming.md).
