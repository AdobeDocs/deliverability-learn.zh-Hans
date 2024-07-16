---
title: Microsoft（Hotmail、Outlook、Windows Live 等）
description: Microsoft一般是第二大或第三大提供商，具体取决于您名单的组成，并且它们的处理流量与其他ISP略有不同。
topics: Deliverability
jira: KT-5319
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---

# [!DNL Microsoft] （[!DNL Hotmail]、[!DNL Outlook]、[!DNL Windows Live]等）

[!DNL Microsoft]一般是第二大或第三大提供商，具体取决于您名单的组成，并且他们的处理流量与其他ISP略有不同。

以下是一些要点：

## 哪些数据重要

[!DNL Microsoft]侧重于发件人信誉、投诉、用户参与度以及他们自己轮询反馈的受信任用户组（也称为发件人信誉数据或SRD）。

## 他们提供哪些数据

[!DNL Microsoft]的专有发件人报告工具[!DNL Smart Network Data Services] (SNDS)允许您查看有关发送多少邮件、接受多少邮件以及投诉和垃圾邮件陷阱的量度。 请记住，共享的数据是一个示例，并不反映确切数字，但它最能表示[!DNL Microsoft]如何将您视为发件人。 [!DNL Microsoft]不公开提供有关其受信任用户组的信息，但可通过[!DNL Return Path Certification]程序获得该数据，但需支付额外费用。

## 发件人信誉

[!DNL Microsoft]传统上在其信誉评估和筛选决策中专注于发送IP。 他们也在积极扩展其发送域功能。 两者在很大程度上都受到投诉和垃圾邮件陷阱等传统信誉影响者的推动。 可投放性还受到回访路径认证计划的严重影响，该计划确实有具体的定量和定性计划要求。

## Insights

[!DNL Microsoft]合并其所有接收域以建立和跟踪发送信誉。 这包括[!DNL Hotmail]、[!DNL Outlook]、MSN、[!DNL Windows Live]等，以及任何公司Office 365托管的电子邮件。 [!DNL Microsoft]可能对卷的波动特别敏感，因此，请考虑应用特定策略从大型发送中提升和降低发送数量，而不是允许基于卷的突然更改。

[!DNL Microsoft]在IP预热的最初几天也特别严格，这通常意味着大多数邮件最初都会被过滤。 大多数ISP认为发件人在被证明有罪之前是无辜的。 [!DNL Microsoft]则相反，在您证明自己无罪之前，会认为您有罪。
