---
title: 基础结构
description: '了解正确构建电子邮件基础架构所需的内容。 '
feature: 过渡流程
topics: Deliverability
kt: 7052
thumbnail: kt7052.jpg
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: d42a8c3b06308fca0cf3e9db8d634a767fc0cdc6
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 0%

---


# 基础结构

能否成功交付取决于坚实的基础。 电子邮件基础架构是核心元素。 一个构造得当的电子邮件基础架构包括多个组件 — 即域和IP地址。 这些组件就像您发送的电子邮件背后的机器，它们通常是发送声誉的锚。 可交付性顾问可确保在实施过程中正确设置这些元素，但由于信誉元素，因此您必须了解这些基本知识。

## 域设置和策略

时代已经改变，一些ISP（如Gmail和雅虎）现在将域名声誉作为附加给发件人电子邮件声誉的附加条件。 您的域信誉基于您的发送域而非您的IP地址。 这意味着在ISP筛选决策方面，您的品牌优先。

Adobe平台上新发件人的入门过程包括设置发送域并确保正确建立基础结构。 您应与专家合作，了解您计划长期使用哪些领域。 下面是形成良好域策略的一些技巧：

* 在您选择的域中尽可能清晰地反映品牌，以便用户不会将邮件错误识别为垃圾邮件。 例如newsletter.foo.com、receets.foo.com等。
* 您不应使用您的父域或公司域，因为它可能会影响组织向ISP发送邮件的投放。
* 考虑使用父域的子域来使发送域合法化。
* 为交易和营销消息类别分离子域。 这将帮助您在更可靠的基础上实现电子邮件流量流，因为ISP会寻找这种发送方法，这是一种已知的电子邮件最佳实践，强烈建议您这样做。

## IP策略

形成结构良好的知识产权战略有助于建立良好声誉，这一点非常重要。 IP的数量和设置因您的业务模式和营销目标而异。 与专家合作，制定明确的开始战略。 请考虑这些需要注意的重要事项：

* **太多的** IPscan会引发声誉问题，因为它是垃圾邮件发送者对雪鞋的常见策略 ****，而雪鞋发送者使用的策略是在许多IP上传播流量，以最大限度地提高垃圾邮件的投放。即使您不是垃圾邮件发送者，如果您使用的IP过多，尤其是那些IP之前没有任何流量时，您看起来可能就像垃圾邮件发送者。
* **IPscan太少会** 导致吞吐量问题，并可能触发信誉问题。吞吐量因ISP而异。 ISP愿意接受多少和多快通常基于其基础架构和发送信誉阈值。
* 为消息类型划分流量是关键。 至少应将营销和交易邮件分别放在不同的IP池上，这一点非常重要。
* 根据您的邮件策略，如果您的信誉大大不同，则最好在不同的IP池上分隔不同的产品或营销流。 部分营销人员还按区域进行细分。 将IP与声誉较低的流量分开不会解决声誉问题，但会防止您的“良好”声誉的电子邮件投放出现问题。 毕竟，你不想牺牲好受众，换个风险更高的。

## 反馈循环

在幕后，Adobe平台正在处理与退回、投诉、取消订阅等相关的数据。 这些反馈循环的设置是可交付性的一个重要方面。 投诉会损害声誉，因此您应该通过电子邮件向目标受众注册投诉。 必须指出的是，Gmail没有提供此数据。 列表取消订阅报头和参与过滤对Gmail用户尤为重要，Gmail用户现在占用户数据库的大部分。

## 身份验证

身份验证是ISP用来验证发送方身份的过程。 最常见的两种身份验证协议是[!DNL Sender Policy Framework](SPF)和[!DNL DomainKeys Identified Mail](DKIM)。 最终用户看不到这些内容，但确实可以帮助ISP过滤来自经过验证的发件人的电子邮件。 [!DNL Domain-based Message Authentication Reporting and Conformance] (DMARC)正在普及，尽管其政策尚未被所有ISP纳入其声誉系统。

### SPF

[!DNL Sender Policy Framework] (SPF)是一种身份验证方法，允许域的所有者指定他们使用哪些邮件服务器从该域发送邮件。

### DKIM

[!DNL Domain Keys Identified Mail] (DKIM)是一种用于检测伪造发件人地址（通常称为欺骗）的身份验证方法。如果启用了DKIM，则允许接收者确认是否授权发件人从该域发送邮件。

### DMARC

[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)是一种身份验证方法，允许域所有者保护其域免遭未经授权的使用。DMARC使用SPF或DKIM，或同时使用DKIM允许域所有者控制对身份验证失败的邮件的操作：已送达、隔离或已拒绝。

## 产品特定资源

**Campaign Standard**

* [控制面板:完全子域委派（教程）](https://experienceleague.corp.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html): *了解如何将子域完全委派给Adobe Campaign Standard。*

**Campaign Classic**

* [控制面板:完全子域委派（教程）](https://experienceleague.corp.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/subdomain-delegation.html): *了解如何将子域完全委派给Adobe Campaign Classic。*
