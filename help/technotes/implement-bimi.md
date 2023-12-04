---
title: 实施Gmail的用于邮件识别的品牌指示器(BIMI)
description: 了解如何实施BIMI
topics: Deliverability
role: Admin
level: Beginner
jira: KT-14079
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: ad0646da88f2b1474e74b6c741d0dd5701e88978
workflow-type: tm+mt
source-wordcount: '1125'
ht-degree: 0%

---

# 实施 [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI)是一种行业标准，它允许在参与平台中的发件人电子邮件旁边显示已批准的徽标。

使用此标准，品牌可以确定应显示在邮箱提供商收件箱中的徽标。 在所谓的BIMI DNS （域名系统）记录中发布后，如果满足某些条件，邮箱提供商可能会挑选此徽标并将其显示在收件箱中。

不同的提供商会进行不同的实施，但好处是显而易见的：站在收件箱中脱颖而出，建立信任，并控制所展示的内容。

BIMI不会直接提高可投放性或您的声誉。 它可以帮助与收件人建立更多信任，从而促进更多参与。

## 它看起来如何？

您可以找到来自不同提供商的一些实施示例，并了解有关哪些提供商的确在网站上显示徽标的更多详细信息 [BIMI组的页面](https://bimigroup.org/where-is-my-bimi-logo-displayed/){target="_blank"}.

## BIMI小组是谁？

BIMI小组是一个开发BIMI的工作组，因为它不仅包括徽标，而且包括技术、法律和合规要求。

BIMI Group由来自业界不同领域的多个利益相关者组成：Google、Yahoo、Fastmail、Proofpoint、Mailchimp、Sendgrid、Valimail和Validity。

## 谁在支持BIMI？

支持BIMI的邮箱提供商名单正在稳步增长。 可以找到最新列表 [此处](https://bimigroup.org/bimi-infographic/){target="_blank"} 支持提供商以及考虑BIMI的提供商。

截至2023年4月，该列表包括Gmail、Yahoo、La Poste、Fastmail、Onet.pl和Zone、Proofpoint作为反垃圾邮件设备以及Apple Mail(从iOS 16开始)。

名单上最显眼的名字显然是雅虎和Gmail，以及最近采用雅虎的Apple和iOS16。 Apple在组合中扮演着特殊角色，因为他们不是邮箱提供商，但他们确实向其本机邮件应用程序添加了BIMI支持。 符合BIMI标准的邮件将显示为“经过数字认证的电子邮件”，以提升对品牌的信任。

## 实施

实施BIMI确实需要几个步骤：

1. DMARC（基于域的消息身份验证、报告和一致性）在发送域及其组织域的执行级别上实施 —  [了解详情](#dmarc)

1. 以SVGTinyPS格式创建您的品牌徽标 —  [了解详情](#create-brand-logo)

1. 注册验证标记证书（仅某些提供商需要） —  [了解详情](#vmc)

1. 发布包含徽标和证书的BIMI DNS记录 —  [了解详情](#publish-bimi-record)

1. 名声很好。 [了解详情](#good-reputation)

>[!NOTE]
>
>请注意，需要勾选所有步骤。


### DMARC {#dmarc}

DMARC是一种标准，它允许品牌决定邮箱提供商应对失败的电子邮件做什么 [身份验证](../additional-resources/authentication.md). 所谓的策略范围从“无”到“隔离”（垃圾邮件文件夹放置）到“拒绝”（完全阻止邮件）。 只有后两项政策被称作“执行”，有资格加入BIMI。 由Adobe发送的邮件正在传递身份验证，因为SPF (Sender Policy Framework)和DKIM (Domain Keys Identified Mail)默认进行了设置。 Adobe正在应请求在您的发送域上设置DMARC。

除了在发送域上使用DMARC之外，还需要在组织域的实施级别使用DMARC(如果发送域是news.example.com，example.com是组织域)。

### 创建您的品牌徽标 {#create-brand-logo}

徽标创建需要完全遵循要求。 请务必参阅 [BIMI小组准则](https://bimigroup.org/creating-bimi-svg-logo-files/){target="_blank"}.

在使用内容分发网络(CDN)时，徽标需要存储在安全位置(HTTPS)中，因此需要禁用阻止邮箱提供商获取徽标的任何保护（例如机器人保护）。

除了技术要求之外，还有一些实用的建议，如采用正方形徽标，背景采用纯色等。 这些推荐有助于实现最佳可视化。 有些提供者有自己的要求，这些要求是除了印巴国际工作组提出的要求之外的。 [Gmail](https://support.google.com/a/answer/10911027?sjid=903725605955621707-EU){target="_blank"}. 例如，要求徽标至少为96 x 96像素。
请注意，不合规可能会导致徽标不显示。

### 已验证标记证书(VMC) {#vmc}

仅某些邮箱提供商(如Gmail和Apple)需要验证标记证书(VMC)，因此是可选的。 我们建议使用VMC来真正利用BIMI。

已验证的标记证书是验证品牌是否可以使用徽标的合法验证。 认证机构将通过注册该品牌标志的商标局对此进行核实。 此过程涉及多次法律验证和检查，可能需要一些时间。 目前有两个CA（证书颁发机构）正在颁发VMC：Digicert和Entrust。 第一组商标办事处为美国、加拿大、欧盟、英国、德国、日本、澳大利亚和西班牙。

一般来说，每个徽标需要一个VMC。 为您的组织域设置一个VMC将涵盖子域，并且其附加功能甚至可以覆盖不同的域。 如果您有不同的徽标，则需要多个VMC。 您选择使用的证书颁发机构或合作伙伴将帮助您进行此设置。

>[!NOTE]
>
>请注意，VMC每年收取费用。

### 发布BIMI记录 {#publish-bimi-record}

完成其他步骤后，即可发布BIMI DNS记录。 记录如下所示：

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

“PEM URL”是已验证标记证书的文件位置。

对于发送域，这需要Adobe完成。

### 良好的声誉 {#good-reputation}

信任是BIMI的关键。 用户将信任其邮箱提供商，以仅向合法发件人显示徽标，因此邮箱提供商需要信任该品牌，而这是由您的发件人信誉完成的。 如果您享有很高的声誉，一切都是好的，但如果您没有声誉，徽标将不会显示。 不幸的是，我们并没有任何信息或指标来判断名声是否足够高。

即使完成VMC的工作和费用也不能消除这一部分。 如果邮箱提供商不信任该品牌，将不会显示徽标。

## 提示和技巧

* BIMI Group为BIMI提供了一个方便的验证工具。 如果您要仔细检查是否一切都已设置并准备就绪，或者只是想查看徽标是否符合要求，请转到 [此链接](https://bimigroup.org/bimi-generator/){target="_blank"}. 对于后者，只需单击 **[!UICONTROL Generate BIMI]** 并输入占位符域但正确的徽标URL。 检查员会告诉您标识是否符合要求。

* 您可以安全地开始使用VMC，如果您的BIMI记录不包含VMC URL，但是该徽标已可在Yahoo中显示，则不会对您的声誉造成损害。

* 在组织层面实施DMARC是一项艰巨的任务。 一些公司专门帮助品牌实现全面的DMARC采用。

* 随后将发布一系列常见问题 [此处](https://bimigroup.org/faqs-for-senders-esps/){target="_blank"}.
