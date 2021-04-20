---
title: 身份验证
description: 了解有关SPF、DKIM和DMARC验证方法的更多信息。
feature: Additional resources
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 0%

---


# 身份验证

## SPF {#spf}

SPF（发送者策略框架）是电子邮件身份验证标准，允许域的所有者指定允许哪些电子邮件服务器代表该域发送电子邮件。 此标准使用电子邮件“回邮路径”标题（也称为“信封发件人”地址）中的域。

>[!NOTE]
>
>可以使用[此外部工具](https://www.kitterman.com/spf/validate.html)验证SPF记录。

SPF是一种技术，在某种程度上，它使您能够确保电子邮件中使用的域名不会伪造。 从域接收消息时，将查询域的DNS服务器。 响应是一条简短记录（SPF记录），详细列出了哪些服务器有权从此域发送电子邮件。 如果我们假设只有域的所有者有更改此记录的方法，我们可以认为此技术不允许伪造发送者地址，至少不允许伪造&quot;@&quot;右侧的部分。

在最终的[RFC 4408规范](https://www.rfc-editor.org/info/rfc4408)中，消息的两个元素用于确定被视为发送方的域：由SMTP &quot;HELO&quot;（或&quot;EHLO&quot;）命令指定的域和由&quot;Return-Path&quot;（或&quot;MAIL FROM&quot;）标头的地址指定的域，也是跳出地址。 不同的考虑使得只考虑其中一个值成为可能；我们建议确保这两个源指定同一域。

检查SPF可评估发件人域的有效性：

* **无**:无法执行任何评估。
* **中立**:查询的域不启用评估。
* **通过**:域被视为正版。
* **失败**:域是伪造的，应拒绝消息。
* **SoftFail**:域可能是伪造的，但不应仅基于此结果拒绝消息。
* **TempError**:临时错误停止了评估。可以拒绝消息。
* **PermError**:域的SPF记录无效。

值得注意的是，在DNS服务器级别所做记录可能需要长达48小时的时间才能考虑到。 此延迟取决于接收服务器的DNS缓存刷新的频率。

## DKIM {#dkim}

DKIM(DomainKeys Indifed Mail)身份验证是SPF的后继身份验证。 它使用公钥密码，允许接收电子邮件服务器验证消息实际上是由其声称其发送消息的个人或实体发送的，以及消息内容在最初发送（和DKIM“签名”）和接收时间之间是否发生了更改。 此标准通常使用“发件人”或“发件人”标题中的域。

DKIM来自DomainKeys， Yahoo! 和Cisco Internet Mail身份验证原则，用于检查发送方域的真实性并保证消息的完整性。

DKIM已替换&#x200B;**DomainKeys**&#x200B;身份验证。

使用DKIM需要一些先决条件：

* **安全性**:加密是DKIM的关键元素。为确保DKIM的安全级别，建议使用1024b作为最佳实践。 大多数访问提供者认为低DKIM密钥无效。
* **声誉**:信誉基于IP和/或域，但较不透明的DKIM选择器也是需要考虑的关键元素。选择选择器很重要：避免保留“违约”，因为“违约”可供任何人使用，因此声誉不佳。 您必须为&#x200B;**保留与客户获取通信**&#x200B;以及身份验证实施不同的选择器。

了解在[本节](/help/additional-resources/acc-technical-recommendations.md#dkim-acc)中使用Campaign Classic时DKIM先决条件的更多信息。

## DMARC {#dmarc}

DMARC(基于域的消息身份验证、报告和符合性)是最新的电子邮件身份验证形式，它依赖SPF和DKIM身份验证来确定电子邮件是否通过或失败。 DMARC在两个重要方面独一无二且功能强大：

* 符合性 — 它允许发送方指示ISP如何处理任何未通过身份验证的消息(例如：不要接受)。
* 报告 — 它向发送方提供详细报告，显示DMARC身份验证失败的所有消息，以及每个用户使用的“发件人”域和IP地址。 这允许公司识别身份验证失败且需要某种类型的“修复”（例如，将IP地址添加到其SPF记录）的合法电子邮件，以及其电子邮件域上的网络钓鱼尝试的来源和流行率。

>[!NOTE]
>
>DMARC可以利用由[250ok](https://250ok.com/)生成的报表。
