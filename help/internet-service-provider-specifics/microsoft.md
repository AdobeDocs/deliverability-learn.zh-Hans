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
TQID: https://experienceleague.adobe.com/pbp5vbHUIHSL9zL9gQf4LpIhEYZI1umEesy3czbJx-4
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
feature_v2:
  - id: ea90ebee-5c84-42d9-8b21-006bdabc95a3
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: f8a45b24-4be7-4f1b-909b-60d06b483a20
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 75df8537199680e5f1fc4b98cefdb05220fee7bf
workflow-type: tm+mt
source-wordcount: 335
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
