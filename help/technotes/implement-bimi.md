---
title: 实施Gmail的用于邮件识别的品牌指示器(BIMI)
description: 了解如何实施BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: aca2bfff9f0315b735cf0a97f2177083c58e0875
workflow-type: tm+mt
source-wordcount: '1062'
ht-degree: 0%

---

# 实施 [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI)是一种行业标准，允许在参与平台中，发送者的电子邮件旁边显示批准的徽标。

使用此标准，品牌可以确定应显示在邮箱提供商收件箱中的徽标。 在所谓的BIMI DNS （域名系统）记录中发布后，如果满足某些条件，邮箱提供商可能会挑选此徽标并将其显示在收件箱中。

不同的提供商会进行不同的实施，但其好处是显而易见的：在收件箱中脱颖而出，建立信任，并控制显示内容。

BIMI不会直接提高可投放性或您的声誉。 它可以帮助与收件人建立更多信任，从而促进更多参与。

## 它看起来如何？

您可以找到来自不同提供商的一些实施示例，以及有关哪些提供商确实在网站上显示徽标的更多详细信息 [BIMI组的页面](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}.

## BIMI小组是谁？

BIMI小组是一个制定BIMI的工作组，因为它不仅包括徽标，而且包括技术、法律和合规要求。

BIMI小组由来自业界不同领域的多个利益相关者组成：Google、Yahoo、Fastmail、Proofpoint、Mailchimp、Sendgrid、Valimail和Validity。

## 谁支持BIMI？

支持BIMI的邮箱提供商名单正在稳步增长。 可以找到最新列表 [此处](https://bimigroup.org/bimi-infographic/){target="_blank"} 支持提供商以及考虑BIMI的提供商。

截至2023年4月，该列表包括Gmail、Yahoo、La Poste、Fastmail、Onet.pl和Zone、Proofpoint作为反垃圾邮件设备以及Apple Mail(从iOS 16开始)。

名单上最显眼的名字显然是雅虎、Gmail和最近采用的一个公司：Apple与iOS 16。 Apple在组合中扮演着特殊角色，因为他们不是邮箱提供商，但他们确实向其本机邮件应用程序添加了BIMI支持。 符合BIMI标准的邮件将显示为“经过数字认证的电子邮件”，这增强了人们对品牌的信任。

## 实施

实施BIMI的确需要几个步骤：

1. DMARC（基于域的消息身份验证、报告和符合性）在发送域及其组织域的执行级别实施 —  [了解详情](#dmarc)

1. 以SVGTinyPS格式创建您的品牌徽标 —  [了解详情](#create-brand-logo)

1. 注册已验证的标记证书（仅某些提供商需要） —  [了解详情](#vmc)

1. 发布包含徽标和证书的BIMI DNS记录 —  [了解详情](#publish-bimi-record)

1. 名声很好。 [了解详情](#good-reputation)

>[!NOTE]
>
>请注意，需要勾选所有步骤。


### DMARC {#dmarc}

DMARC是一种标准，允许品牌决定邮箱提供商应对失败的电子邮件做什么 [身份验证](../additional-resources/authentication.md). 所谓的策略范围从“无”到“隔离”（垃圾邮件文件夹放置）到“拒绝”（完全阻止邮件）。 只有后两项政策被称为“执行”，有资格加入BIMI。 由Adobe发送的邮件正在传递身份验证，因为SPF (Sender Policy Framework)和DKIM (Domain Keys Identified Mail)是默认设置的。 Adobe正在应请求在您的发送域上设置DMARC。

除了在发送域上使用DMARC之外，还需要在组织域的实施级别上使用DMARC(如果发送域是news.example.com，example.com是组织域)。

### 创建您的品牌徽标 {#create-brand-logo}

徽标创建需要完全遵循要求。 请务必参阅 [BIMI小组准则](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}.

除了技术要求之外，还有一些实用的建议，如使用方形徽标、使用纯色作为背景等等。 这些推荐有助于实现最佳可视化。
请注意，不合规可能会导致徽标不显示。

### 已验证标记证书(VMC) {#vmc}

只有Gmail和Apple等邮箱提供商才需要验证标记证书(VMC)，因此它是可选的。 我们建议使用VMC来真正利用BIMI。

已验证的标记证书是合法验证，表明品牌可以使用徽标。 认证机构将通过注册该品牌标志的商标局进行核实。 此过程涉及多个法律验证和检查，可能需要一些时间。 目前有两个CA（证书颁发机构）正在颁发VMC：Digicert和Entrust。 第一组商标办事处是美国、加拿大、欧盟、英国、德国、日本、澳大利亚和西班牙。

一般来说，每个徽标需要一个VMC。 为您的组织域设置一个VMC将覆盖子域，而且即使在不同域中也会增加一项功能。 如果您有不同的徽标，则需要多个VMC。 您选择使用的证书颁发机构或合作伙伴将帮助您进行此设置。

>[!NOTE]
>
>请注意，VMC每年收取费用。

### 发布BIMI记录 {#publish-bimi-record}

完成其他步骤后，可以发布BIMI DNS记录。 记录如下所示：

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

“PEM URL”是已验证标记证书的文件位置。

对于发送域，这需要Adobe完成。

### 良好的声誉 {#good-reputation}

信任是BIMI的关键。 用户将信任其邮箱提供商以仅显示合法发件人的徽标，因此邮箱提供商需要信任该品牌，而这是由您的发件人信誉完成的。 如果您享有很高的声誉，则一切都是好的，但是如果您没有声誉，则徽标将不会显示。 不幸的是，我们并没有任何信息或指标可以用来判断名声是否足够高。

即使完成VMC的工作和开支也不会消除这一部分。 如果邮箱提供商不信任该品牌，将不会显示徽标。

## 提示和技巧

* BIMI Group为BIMI提供了一个方便的验证工具。 如果您要仔细检查所有项目是否已设置并准备就绪，或者只是想查看徽标是否符合，请转到 [此链接](https://bimigroup.org/bimi-generator/){target="_blank"}. 对于后者，只需单击 **[!UICONTROL Generate BIMI]** 并输入占位符域，但需输入正确的徽标URL。 检查员会告诉您标识是否符合要求。

* 您可以安全地开始使用VMC，如果您的BIMI记录不包含VMC URL，但是该徽标已可在Yahoo中显示，则不会损害您的声誉。

* 在组织层面实施DMARC是一项大工程。 一些公司专门帮助品牌实现全面采用DMARC。

* 随后将发布一系列常见问题 [此处](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}.
