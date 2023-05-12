---
title: 实施Gmail的品牌标识报文(BIMI)
description: 了解如何实施BIMI
topics: Deliverability
exl-id: 6b911bcc-a531-466a-8bd3-7fa469b96cc7
source-git-commit: 05f6cd331f4e610e2442d43405333823644d349e
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 0%

---

# 实施 [!DNL Brand Indicators for Message Identification] (BIMI)

[!DNL Brand Indicators for Message Identification] (BIMI)是一个行业标准，允许在参与的平台中，在发件人电子邮件旁边显示一个经过批准的徽标。

使用此标准，品牌可以确定应显示在邮箱提供商收件箱中的徽标。 一旦在所谓的BIMI DNS（域名系统）记录中发布，邮箱提供商可能会选取此徽标，并在符合特定条件时将其显示在收件箱中。

不同的提供商执行不同的实施，但好处是显而易见的：站在收件箱中，构建信任并控制所显示内容。

BIMI不会直接提高投放能力或声誉。 它有助于与收件人建立更多的信任，从而提高参与度。

## 看起来怎么样？

您可以找到来自不同提供商的实施的一些示例，以及有关哪些提供商在 [BIMI Group的页面](https://bimigroup.org/where-is-my-bimi-logo-displayed/).

## BIMI集团是谁？

BIMI小组是开发BIMI的工作组，因为它不仅涵盖标识，还涵盖技术、法律和合规要求。

BIMI集团由来自行业不同领域的若干利益相关方组成：Google、Yahoo、Fastmail、Proofpoint、Mailchimp、Sendgrid、Valimail和Validity。

## 谁支持BIMI?

支持BIMI的邮箱提供商名单正在稳步增长。 可以找到最新列表 [此处](https://bimigroup.org/bimi-infographic/) 支持提供商和考虑BIMI的提供商。

自2023年4月起，该列表包括Gmail、Yahoo、La Poste、Fastmail、Onet.pl和Zone、Proofpoint作为防垃圾邮件设备和Apple Mail(从iOS 16开始)。

该名单上最知名的名字显然是雅虎、Gmail，以及最近的一位采用者：Apple和iOS16。 Apple在此组合中发挥了特殊作用，因为它们不是邮箱提供商，但是它们确实在其本机邮件应用程序中添加了BIMI支持。 与BIMI兼容的邮件将显示为“经过数字认证的电子邮件”，这将提高对品牌的信任。

## 实施

实施BIMI确实需要采取以下几个步骤：

1. 在发送域及其组织域的执行级别上实施DMARC（基于域的消息身份验证、报告和符合性） —  [了解更多](#dmarc)

1. 以SVGTinyPS格式创建品牌徽标 —  [了解更多](#create-brand-logo)

1. 注册已验证的标记证书（仅某些提供商需要） —  [了解更多](#vmc)

1. 使用徽标和证书发布BIMI DNS记录 —  [了解更多](#publish-bimi-record)

1. 名声不错。 [了解更多](#good-reputation)

>[!NOTE]
>
>请注意，需要勾选所有步骤。


### DMARC {#dmarc}

DMARC是一个标准，它允许品牌决定邮箱提供商对失败的电子邮件应采取什么措施 [身份验证](../additional-resources/authentication.md). 所谓的策略范围从“无”到“隔离”（垃圾邮件文件夹放置），再到“拒绝”（直接阻止邮件）。 只有后两种政策被称为&quot;执行&quot;，并符合BIMI的资格。 由Adobe发送的邮件正在传递身份验证，因为SPF（发件人策略框架）和DKIM（域名标识邮件）是默认设置的。 Adobe正在根据请求在您的发送域上设置DMARC。

除了发送域上的DMARC之外，还需要在组织域的执行级别使用DMARC（如果发送域是news.example.com， example.com是组织域）。

### 创建品牌徽标 {#create-brand-logo}

徽标的创建需要完全遵循要求。 请始终参考 [BIMI Group的准则](https://bimigroup.org/creating-bimi-svg-logo-files/).

除技术要求外，还有一些实用的建议，如标有方形标识、以纯色为背景等。 这些推荐是最佳可视化图表。
请注意，不合规可能会导致徽标不显示。

### 已验证标记证书(VMC) {#vmc}

只有Gmail和Apple等某些邮箱提供商才需要验证标记证书(VMC)，因此它是可选的。 不过，我们确实建议使用VMC来真正利用BIMI。

经验证的标记证书是品牌可以使用徽标的法律验证。 认证机构将通过注册该品牌标志的商标局进行检查。 此过程涉及多项法律验证和检查，并且可能需要一些时间。 目前，有两个CA（证书颁发机构）正在颁发VMC:迪吉克特和Entrust。 首批商标办事处是美国、加拿大、欧盟、英国、德国、日本、澳大利亚和西班牙。

根据经验，每个徽标需要一个VMC。 为组织域设置VMC将涵盖子域，而且即使是不同域，还添加了一项功能。 如果您的徽标不同，则需要多个VMC。 您选择与之合作的认证机构或合作伙伴将帮助您进行此设置。

>[!NOTE]
>
>请注意，VMC需要按年收费。

### 发布BIMI记录 {#publish-bimi-record}

完成其他步骤后，即可发布BIMI DNS记录。 记录如下所示：

```
default._bimi.[domain] IN TXT "v=BIMI1; l=[SVG URL]; a=[PEM URL]
```

“PEM URL”是已验证标记证书的文件位置。

对于您的发送域，需要通过Adobe来完成。

### 声誉良好 {#good-reputation}

信任是BIMI的关键。 用户信任其邮箱提供商，只显示合法发件人的徽标，因此邮箱提供商需要信任该品牌，这是由您的发件人信誉完成的。 如果您声誉很高，一切都很好，但如果您没有，徽标将不会显示。 遗憾的是，我们无法查看任何信息或量度来判断声誉是否足够高。

即使经过VMC的努力和费用，也不会夺走这一部分。 如果邮箱提供商不信任该品牌，则不会显示徽标。

## 提示和技巧

* BIMI小组为BIMI提供了方便的验证工具。 如果您想要再次检查所有内容是否已设置并准备就绪，或者只想查看徽标是否符合标准，请转到 [此链接](https://bimigroup.org/bimi-generator/). 对于后者，只需单击 **[!UICONTROL Generate BIMI]** 并输入占位符域，但是正确的徽标URL。 检查员会告诉你徽标是否合规。

* 您无需VMC即可安全启动，如果您的BIMI记录不包含VMC URL，但该徽标已显示在Yahoo中，则对您的声誉没有任何损害。

* 在组织层面实施DMARC是一项大工程。 一些公司专门帮助品牌全面采用DMARC。

* 发布了一系列常见问题 [此处](https://bimigroup.org/faqs-for-senders-esps/).
