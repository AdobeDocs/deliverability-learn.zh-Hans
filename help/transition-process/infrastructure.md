---
title: 基础结构
description: '了解需要什么才能正确地构造电子邮件基础架构。 '
topics: Deliverability
kt: 7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 4025d95c-cc77-4e0c-9904-aaf60019b18c
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 2%

---

# 基础结构

能否成功投放取决于坚实的基础。 电子邮件基础结构是核心元素。 正确构建的电子邮件基础结构包括多个组件，即域和IP地址。 这些组件与您发送的电子邮件背后的机器类似，通常也是发送声誉的锚。 可投放性顾问可确保在实施过程中正确设置这些元素，但鉴于信誉元素，您务必要了解这些基本信息。

## 域设置和策略 {#domain-setup-and-strategy}

时代已经改变，一些ISP（如Gmail和Yahoo）现在将域名声誉作为附加到发件人电子邮件声誉上的附加点。 您的域名信誉基于您的发送域，而不是IP地址。 这意味着在ISP筛选决策时，您的品牌优先受用。

在Adobe平台上为新发件人提供的入门流程包括设置发送域并确保正确建立基础结构。 您应该与专家合作，了解您计划长期使用哪些领域。 以下是一些可形成良好域策略的提示：

* 在您选择的域中尽可能清晰地反映品牌，以便用户不会将邮件错误地识别为垃圾邮件。 有些示例包括newsletter.foo.com、reects.foo.com等。
* 您不应使用父域或公司域，因为它可能会影响从贵组织向ISP发送邮件。
* 考虑使用父域的子域来使发送域合法化。
* 为事务型消息和营销消息类别分隔子域。 这将帮助您的电子邮件流量在更可靠的基础上流动，因为ISP会查找此发送方法，这是已知的电子邮件最佳实践，强烈建议使用此方法。

## IP策略 {#ip-strategy}

制定结构完善的知识产权战略以帮助建立良好的声誉，这一点很重要。 IP和设置的数量因您的业务模式和营销目标而异。 与专家合作，制定明确的战略，以便立即开始。 请考虑以下需要注意的事项：

* **太多的** IPscan会触发声誉问题，因为它是垃圾邮件发送者对Snowshoe的常见策略，而Snowshoe ****&#x200B;是垃圾邮件发送者使用的一种策略，其中流量分布在许多IP中，以最大限度地发送垃圾邮件。即使您不是垃圾邮件发送者，如果您使用的IP过多，特别是这些IP之前没有任何流量，您看起来可能就像一个垃圾邮件发送者。
* **IPscan太少会** 导致吞吐量问题，并可能触发信誉问题。吞吐量因ISP而异。 ISP愿意接受的数量和速度通常取决于其基础架构和发送信誉阈值。
* 为报文传送类型划分流量是关键。 至少应在单独的IP池上单独分开营销和事务型邮件。
* 根据您的邮件策略，如果您的声誉存在显着差异，则建议在不同的IP池上分隔不同的产品或营销流。 某些营销人员还按地区进行细分。 为声誉较低的流量分隔IP不会解决声誉问题，但会防止“声誉良好的”电子邮件投放出现问题。 毕竟，你不想为了一个风险更高的受众而牺牲好观众。

## 反馈循环 {#feedback-loops}

在后台，Adobe平台正在处理有关退回、投诉、取消订阅等的数据。 这些反馈循环的设置是交付性的一个重要方面。 投诉可能会损害声誉，因此您应该通过电子邮件地址来注册目标受众的投诉。 请务必注意，Gmail不会提供此数据。 列出取消订阅标头和参与度过滤对Gmail订阅者尤其重要，如今，Gmail订阅者已占据大多数订阅者数据库。

## 身份验证 {#authentication}

身份验证是ISP用于验证发件人身份的过程。 最常见的两种身份验证协议是[!DNL Sender Policy Framework](SPF)和[!DNL DomainKeys Identified Mail](DKIM)。 最终用户看不到这些邮件，但可帮助ISP过滤来自已验证发件人的电子邮件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)越来越受欢迎，尽管其政策尚未被所有ISP纳入其声誉系统。

### SPF

[!DNL Sender Policy Framework] (SPF)是一种身份验证方法，允许域的所有者指定用于从该域发送邮件的邮件服务器。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一种身份验证方法，用于检测伪造的发件人地址（通常称为欺骗）。如果启用了DKIM，则允许接收者确认是否有权从该域发送邮件。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一种验证方法，允许域所有者保护其域免受未经授权的使用。DMARC使用SPF或DKIM或两者都允许域所有者控制验证失败邮件的情况：已送达、已隔离或已拒绝。

## 产品特定资源

**Campaign**

* 了解如何在[此部分](/help/additional-resources/ac-domain-name-setup.md)中将子域完全委派给Adobe Campaign Classic或Standard。
* [控制面板:完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html)  -  *了解如何将子域完全委派给Adobe Campaign Classic。*
* [控制面板:完全子域委派（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html)  -  *了解如何将子域完全委派给Adobe Campaign Standard。*
* 在[此部分](/help/additional-resources/acc-technical-recommendations.md#feedback-loop-acc)中了解有关为Campaign Classic实例实施反馈循环的更多信息。

## 其他资源

* 在[此部分](/help/additional-resources/authentication.md)中了解有关SPF、DKIM和DMARC身份验证方法的更多信息。
* 在[此部分](/help/additional-resources/increase-reputation-with-ip-warming.md)中了解有关通过IP变温提高电子邮件声誉的更多信息。
