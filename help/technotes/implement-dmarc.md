---
title: 实施Gmail的用于邮件识别的品牌指示器(BIMI)
description: 了解如何实施BIMI
topics: Deliverability
role: Admin
level: Beginner
exl-id: f1c14b10-6191-4202-9825-23f948714f1e
TQID: https://experienceleague.adobe.com/gvO7rHqY-Dm6nUq9ssccY7Kt1xobV-pqLc26iITgmRA
product_v2: id: b27e5950-9033-45ac-9f86-eb22e567f615id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2: id: e2290edd-b061-4880-9d79-dee306cf5aa9id: ea90ebee-5c84-42d9-8b21-006bdabc95a3id: f71e690b-4480-4b67-9ef5-88f42f9cdfdbid: f82558ea-6af5-44eb-a424-5b3389abb0a3
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 1326
ht-degree: 9%

---

# 实施[!DNL Domain-based Message Authentication, Reporting and Conformance] (DMARC)

本文档旨在为读者提供有关电子邮件身份验证方法DMARC的更多信息。 通过说明DMARC的工作方式及其各种策略选项，读者将更好地了解DMARC对电子邮件投放能力的影响。

## 什么是DMARC？ {#about}

Domain-based Message Authentication, Reporting and Conformance是一种电子邮件身份验证方法，它允许域所有者保护其域免遭未经授权的使用。 DMARC还提供有关电子邮件身份验证状态的反馈，并允许发件人控制身份验证失败的电子邮件发生的情况。 这包括根据已实施的DMARC策略监视、隔离或拒绝邮件的选项。

DMARC有三个策略选项：

* **监视器(p=none)：**&#x200B;指示邮箱提供商/ISP执行通常对邮件执行的操作。
* **隔离（p=隔离）：**&#x200B;指示邮箱提供商/ISP传送未将DMARC传递到收件人的垃圾邮件或垃圾邮件文件夹的邮件。
* **拒绝(p=reject)：**&#x200B;指示邮箱提供商/ISP阻止未通过DMARC的邮件，从而导致退回。

## DMARC的工作原理 {#how}

SPF和DKIM都用于关联电子邮件和域，并共同验证电子邮件。 DMARC更进一步，通过匹配DKIM和SPF检查的域，帮助防止欺骗。 要传递DMARC，消息必须传递SPF或DKIM。 如果这两项身份验证均失败，DMARC将失败，并根据您选择的DMARC策略发送电子邮件。

>[!NOTE]
>
>DMARC要求在“发件人”和“返回路径”地址之间保持一致。

## 为什么要实施DMARC？ {#why}

DMARC是可选的，虽然并非必需，但它免费，允许电子邮件接收者轻松识别电子邮件的身份验证，这可能会改善投放。 DMARC的主要优势之一是，它可以报告哪些消息未通过SPF和/或DKIM。 它还可以使发件人在一定程度上控制未通过上述任一身份验证方法的邮件所发生的情况。 通过DMARC报告，发件人可了解哪些消息在DMARC中失败，从而采取相应步骤来减少进一步的错误。

>[!NOTE]
>
>如果要实施BIMI，则需要p=quarantine或p=reject DMARC策略。

## 实施DMARC的最佳实践 {#best-practice}

由于DMARC是可选的，因此默认情况下，不会在任何ESP平台上对其进行配置。 必须在DNS中为您的域创建DMARC记录才能使其正常工作。 此外，需要您选择的电子邮件地址来指示DMARC报表在贵组织内的放置位置。 作为最佳实践，它是
建议您将DMARC策略从p=none提升到p=quarantine再提升到p=reject，以慢慢推出DMARC实施，因为DMARC已经了解DMARC的潜在影响。

1. 分析您收到并使用的反馈(p=none)，这告知接收者不对身份验证失败的邮件执行任何操作，但仍会向发件人发送电子邮件报告。 此外，如果合法邮件未通过身份验证，则查看和修复 SPF/DKIM 的问题。
1. 确定SPF和DKIM是否一致并通过所有合法电子邮件的身份验证，然后将策略移至(p=quarantine)，这会告知接收电子邮件服务器隔离身份验证失败的电子邮件（这通常意味着将这些邮件放入垃圾邮件文件夹）。
1. 将策略调整为（p=拒绝）。 p= reject 策略告知接收者完全拒绝（退回）验证失败的域的所有电子邮件。 启用此策略后，只有经域验证为 100% 经过身份验证的电子邮件才有机会进入收件箱。

   >[!NOTE]
   >
   >请谨慎使用此策略，并确定它是否适合您的组织。

## DMARC报表 {#reporting}

DMARC提供接收未通过SPF/DKIM的电子邮件报表的功能。 在身份验证过程中，ISP服务生成了两个不同的报告，发件人可以通过其DMARC策略中的RUA/RUF标记接收这些报告：

* **汇总报表(RUA)：**&#x200B;不包含任何对GDPR敏感的PII（个人身份信息）。
* **取证报告(RUF)：**&#x200B;包含对GDPR敏感的电子邮件地址。 在使用之前，最好在内部检查如何处理需要符合GDPR的信息。

这些报告的主要用途是接收尝试欺骗的电子邮件概述。 这些是技术含量很高的报告，最好通过第三方工具消化。 一些专门从事DMARC监控的公司包括：

* [ValiMail](https://www.valimail.com/products/#automated-delivery)
* [阿加里语](https://www.agari.com/)
* [德马尔西安](https://dmarcian.com/)
* [校对](https://www.proofpoint.com/us)

>[!CAUTION]
>
>如果要添加用于接收报告的电子邮件地址位于为其创建 DMARC 记录的域之外，则需要授权其外部域以指定到您拥有此域的 DNS。 为此，请执行 [dmarc.org 文档](https://dmarc.org/2015/08/receiving-dmarc-reports-outside-your-domain)中详述的步骤

### DMARC记录示例 {#example}

```
v=DMARC1; p=reject; fo=1; rua=mailto:dmarc_rua@emaildefense.proofpoint.com;ruf=mailto:dmarc_ruf@emaildefense.proofpoint.co
```

## DMARC标记及其用途 {#tags}

DMARC记录具有多个名为DMARC标记的组件。 每个标记都有一个值，该值指定DMARC的某些方面。

| 标记名称 | 必需/可选 | 函数 | 示例 | 默认值 |
|  ---  |  ---  |  ---  |  ---  |  ---  |
| v | 必需 | 此DMARC标记指定版本。 目前只有一个版本，因此其固定值为v=DMARC1 | V=DMARC1 DMARC1 | DMARC1 |
| p | 必需 | 显示选定的DMARC策略，并指示接收者报告、隔离或拒绝未通过身份验证检查的邮件。 | p=none、quarantine或reject | - |
| fo | 可选 | 允许域所有者指定报告选项。 | 0：如果所有失败都生成报告<br/>1：如果任何失败都生成报告<br/>d：如果DKIM失败则生成报告<br/>s：如果SPF失败则生成报告 | 1（建议用于DMARC报表） |
| pct | 可选 | 告知受过滤的邮件的百分比。 | pct=20 | 100 |
| rua | 可选（推荐） | 标识将提交汇总报表的位置。 | `rua=mailto:aggrep@example.com` | - |
| ruf | 可选（推荐） | 确定将提交鉴证报告的位置。 | `ruf=mailto:authfail@example.com` | - |
| sp | 可选 | 为父域的子域指定DMARC策略。 | sp=reject | - |
| adkim | 可选 | 可以是严格(s)或宽松(r)。 宽松的对齐意味着DKIM签名中使用的域可以是“发件人”地址的子域。 严格对齐意味着DKIM签名中使用的域必须与发件人地址中使用的域完全匹配。 | adkim=r | r |
| aspf | 可选 | 可以是严格(s)或宽松(r)。 宽松的对齐意味着ReturnPath域可以是From Address的子域。 严格对齐意味着Return-Path域必须与From地址完全匹配。 | aspf=r | r |

## DMARC和Adobe Campaign {#campaign}

>[!NOTE]
>
>如果您的Campaign实例托管在AWS上，则可以使用该控制面板为子域实施DMARC。 [了解如何使用控制面板](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/txt-records/dmarc.html)实施DMARC记录。

DMARC失败的常见原因是“发件人”和“错误收件人”或“返回路径”地址之间未对齐。 为避免这种情况，在设置DMARC时，建议仔细检查投放模板中的“发件人”和“错误收件人”地址设置。

1. 在您的投放模板中，查看当前设置为您的“发件人”地址的地址。

   ![](../assets/dmarc1.png)

1. 从这里，选择“属性”，这将允许您进一步编辑投放模板。 在此窗口中，选择SMTP，如果选中，请取消选中“使用为平台定义的默认错误地址”。 Adobe Campaign中的投放模板默认选中此复选框。 默认错误地址可能不是与此投放模板中的发件人地址关联的地址。

   ![](../assets/dmarc2.png)

1. 如果未选中此框，则会显示一个文本字段，允许您输入唯一的错误地址，该地址使用与在“发件人地址”中设置的域相同的域。

   ![](../assets/dmarc3.png)

保存这些更改后，您就可以在正确的域对齐的情况下继续实施DMARC。

## 有用链接 {#links}

* [DMARC.org](https://dmarc.org/){target="_blank"}
* [M3AAWG电子邮件身份验证](https://www.m3aawg.org/sites/default/files/document/M3AAWG_Email_Authentication_Update-2015.pdf){target="_blank"}
