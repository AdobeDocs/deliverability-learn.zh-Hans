---
title: 身份验证
description: 了解有关SPF、DKIM和DMARC身份验证方法的更多信息。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 03609139-b39b-4051-bcde-9ac7c5358b87
TQID: https://experienceleague.adobe.com/zuhBmNWmF8CoCSNofsg3FKCcQFLOFfZmRutB2P1L4-U
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2: id: ea90ebee-5c84-42d9-8b21-006bdabc95a3id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 769
ht-degree: 0%

---

# 身份验证

## SPF {#spf}

SPF （发件人策略框架）是一种电子邮件身份验证标准，它允许域的所有者指定允许哪些电子邮件服务器代表该域发送电子邮件。 此标准使用电子邮件的“Return-Path”标头（也称为“Envelope From”地址）中的域。

>[!NOTE]
>
>您可以使用[此外部工具](https://www.kitterman.com/spf/validate.html)验证SPF记录。

SPF是一种技术，在某种程度上，它使您能够确保电子邮件中使用的域名不被伪造。 当从域接收消息时，将查询域的DNS服务器。 响应是一个简短记录（SPF记录），详细说明哪些服务器有权从此域发送电子邮件。 如果我们假设只有域的所有者才有办法更改此记录，则可以认为此技术不允许伪造发件人地址，至少不允许伪造来自“@”右侧的部分。

在最终的[RFC 4408规范](https://www.rfc-editor.org/info/rfc4408)中，使用邮件的两个元素来确定被视为发件人的域：由SMTP“HELO”（或“EHLO”）命令指定的域，以及由“Return-Path”（或“MAIL FROM”）标头（也是退回地址）的地址指定的域。 不同的考虑因素使得仅考虑这些值之一成为可能；我们建议确保两个源都指定相同的域。

检查SPF可以评估发件人域的有效性：

* **无**：无法执行评估。
* **Neutral**：查询的域未启用评估。
* **通过**：域被视为真实域。
* **失败**：域被伪造，邮件应被拒绝。
* **SoftFail**：域可能是伪造的，但不应仅基于此结果拒绝该消息。
* **TempError**：临时错误停止了评估。 可以拒绝该消息。
* **PermError**：域的SPF记录无效。

值得一提的是，在DNS服务器级别所做的记录最多需要48小时才能考虑在内。 此延迟取决于接收服务器的DNS缓存刷新频率。

## DKIM {#dkim}

DKIM (DomainKeys Indified Mail)身份验证是SPF的后继身份验证。 它使用公钥加密技术，允许接收电子邮件服务器验证消息实际上是由其声称发送该消息的个人或实体发送的，以及消息内容在最初发送时间（和DKIM“签名”）与接收时间之间是否进行了更改。 此标准通常使用“发件人”或“发件人”标头中的域。

DKIM来自域名组合Yahoo！ 和Cisco Identified Internet Mail验证原则，用于检查发送者域的真实性并确保消息的完整性。

DKIM已替换&#x200B;**DomainKeys**&#x200B;身份验证。

使用DKIM需要一些先决条件：

* **安全性**：加密是DKIM的密钥元素。 为确保DKIM的安全级别，1024b是推荐的最佳实践加密大小。 大多数访问提供商都不认为较低级别的DKIM密钥有效。
* **信誉**：信誉基于IP和/或域，但不太透明的DKIM选择器也是要考虑的关键元素。 选择选择器非常重要：避免保留可供任何人使用、因此信誉不佳的“默认”选择器。 您必须为&#x200B;**保留期与客户获取通信**&#x200B;以及身份验证实施不同的选择器。

在[本节](/help/additional-resources/acc-technical-recommendations.md#dkim-acc)中了解使用DKIM时的Campaign Classic先决条件的更多信息。

## DMARC {#dmarc}

DMARC (Domain-based Message Authentication, Reporting and Conformance)是电子邮件身份验证的最新形式，它依赖于SPF和DKIM身份验证来确定电子邮件通过还是失败。 DMARC独一无二，功能强大，它体现在两个重要方面：

* 符合性 — 它允许发件人指示ISP如何处理无法验证的任何消息（例如：不接受它）。
* 报告 — 它向发件人提供详细的报告，其中显示所有未通过DMARC身份验证的消息，以及每个消息使用的“发件人”域和IP地址。 这允许公司识别未通过身份验证且需要某种“修复”类型的合法电子邮件（例如，将IP地址添加到其SPF记录），以及在其电子邮件域上发起网络钓鱼攻击的来源和普遍程度。

>[!NOTE]
>
>DMARC可以利用[250ok](https://250ok.com/)生成的报告。
