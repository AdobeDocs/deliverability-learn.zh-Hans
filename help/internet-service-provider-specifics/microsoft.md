---
title: Microsoft（Hotmail、Outlook、Windows Live 等）
description: Microsoft一般是第二大或第三大提供商，具体取决于您名单的组成，并且它们处理的流量与其他ISP略有不同。
topics: Deliverability
jira: KT-5319
doc-type: article
activity: understand
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 2%

---

# [!DNL Microsoft] ([!DNL Hotmail]， [!DNL Outlook]， [!DNL Windows Live]、等)

[!DNL Microsoft] 通常为第二大或第三大提供商（具体取决于您名单的组成），它们的处理流量与其他ISP略有不同。

以下是一些亮点：

## 哪些数据非常重要

[!DNL Microsoft] 重点关注发件人信誉、投诉、用户参与以及他们自己负责轮询反馈的受信任用户组（也称为发件人信誉数据或SRD）。

## 他们提供了哪些数据

[!DNL Microsoft]的专有发件人报告工具， [!DNL Smart Network Data Services] (SNDS)，可让您查看关于发送多少邮件、接受多少邮件以及投诉和垃圾邮件陷阱的量度。 请记住，共享的数据是一个示例，并不反映准确的数字，但最能说明如何这样做 [!DNL Microsoft] 将您视为发件人。 [!DNL Microsoft] 不公开提供有关其受信任用户组的信息，但可通过 [!DNL Return Path Certification] 计划需额外付费。

## 发件人信誉

[!DNL Microsoft] 传统上，他们专注于在信誉评估和筛选决策中发送IP。 他们也在积极扩展其发送域功能。 两者主要受到投诉和垃圾邮件陷阱等传统信誉影响者的推动。 可交付性还受到回访路径认证计划的严重影响，该计划确实有具体的定量和定性计划要求。

## 见解

[!DNL Microsoft] 合并其所有接收域以建立和跟踪发送信誉。 这包括 [!DNL Hotmail]， [!DNL Outlook]，MSN， [!DNL Windows Live]，等等，以及任何企业Office 365托管的电子邮件。 [!DNL Microsoft] 可能对数量的波动特别敏感，因此请考虑应用特定策略来增大和减小大型发送的数量，而不是允许基于数量的突然更改。

[!DNL Microsoft] 此外，在IP预热的最初几天特别严格，这通常意味着大多数邮件最初都会被过滤。 大多数ISP认为发件人在被证明有罪之前是无辜的。 [!DNL Microsoft] 恰恰相反，在你证明自己无罪之前，会认为你有罪。
