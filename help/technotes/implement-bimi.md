---
title: 实施Gmail的品牌标识报文(BIMI)
description: 了解如何实施BIMI
topics: Deliverability
hide: true
hidefromtoc: true
source-git-commit: ab1595bac7ef136eb001609b9017950a2d01cbb4
workflow-type: tm+mt
source-wordcount: '698'
ht-degree: 0%

---


# 实施Gmail的品牌标识报文(BIMI)

Gmail最近宣布将[推出BIMI](https://cloud.google.com/blog/products/identity-security/bringing-bimi-to-gmail-in-google-workspace)的一般支持。 在利用此功能之前，您必须处理许多项目，包括：已验证标记证书、商标标志、格式正确的标志、DMARC设置，并最终将BIMI记录发布到您的DNS。 我们将在本文中回顾所有这些步骤。

报文识别的品牌指示器(BIMI)是一个行业标准，允许在参与的平台中，在发件人电子邮件旁边显示一个经过批准的徽标。 这种吸引眼球的做法不仅可能提高参与度，还有助于确认发送者的真实性，从而降低网络钓鱼和其他垃圾邮件策略的风险。

## 已验证标记证书

Gmail BIMI程序的一个关键组成部分是要求发件人拥有由有效证书颁发机构颁发的经验证的标记证书(VMC)。 目前，这些VMC只能从Entrust和DigiCert获得，但在Gmail发布公告后，这些提供商的名单可能会增长。

VMC在某些方面将与SSL证书类似。 对于要显示的每个徽标，您需要一个VMC，因此，如果您有许多品牌，您应当计划需要多个VMC。 每个VMC在多个域中都有效，但是如果您获得多SAN VMC。 因此，如果您希望一个徽标跨多个发送域显示，则只需要一个VMC。

## 商标标识

在获取VMC之前，还有一个关键步骤必须完成：要获得VMC，您要显示的徽标必须在8个已获批准的全球商标和专利局之一注册。

* 美国专利和商标局(USPTO)
* 加拿大知识产权局
* 欧盟知识产权局
* 英国知识产权局
* 德国专利与市场主义
* 日本商标局
* 西班牙专利和商标局
* IP澳大利亚

如果您要显示的徽标未注册，或未在这8个组织中的某一个组织注册，则您需要与您的法律团队合作以解决该问题，然后才能申请VMC。

## 徽标图像格式

这也是确保您的徽标符合BIMI徽标格式要求的好时机。 必须采用SVG格式，并遵循SVG可移植/安全(SVG-P/S)配置文件。 有关如何这样做的指导可在[BIMI工作组](https://bimigroup.org/svg-conversion-tools-released)中找到。

## DMARC

在您的商标徽标格式正确，并且已验证的标记证书格式正确后，您还需要确保在您希望BIMI工作的任何发送域上已完全配置DMARC。

这包括确保将P=设置为隔离或拒绝。 如果您的DMARC使用P=None，则它将不符合BIMI的资格。 强烈建议使用P=None设置，以确保您知道域中传出的邮件，并且如果您更改为“隔离”或“拒绝”，则不会意外阻止任何邮件，请将其视为测试和信息收集阶段。 在BIMI可供您使用之前，您需要超越这一限制进入执行阶段。

## DNS条目

其他所有内容最终排好队，准备就绪，现在该用您的BIMI更新DNS条目了。

这是一个简单的条目，应当如下所示：

```
default._bimi.[domain] IN TXT “v=BIMI1; l=[SVG URL] 
```

您可以获取有关该条目的详细信息，甚至可以在[BIMI工作组站点](https://bimigroup.org/implementation-guide)使用免费的BIMI检查程序。


## 主要优点

如果您是Adobe Campaign或Marketo客户，Adobe可帮助您创建BIMI DNS更新：请联系Adobe客户关怀团队以请求获取。 Adobe还有助于对BIMI不能正常工作的情况进行故障诊断。

要获得有关商标或经验证的标记证书的帮助，请与您的法律团队和授权的VMC供应商合作。

为Gmail设置BIMI可能不是一个快速的过程，但从营销和安全角度来看，这个过程可以带来显着好处。
