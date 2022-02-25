---
title: Verizon Media Group（Yahoo、AOL、Verizon 等）
description: '"[!DNL Verizon Media Group] 通常是大多数B2C列表的前三个域之一。 他们的行为有些独特，因为如果声誉出现问题，他们通常会限制或批量发送邮件。”'
topics: Deliverability
kt: 5320
doc-type: article
activity: understand
team: TM
exl-id: 43e6d3cb-23c3-4076-8026-a1a08e76bd1b
source-git-commit: a5c86d5e6f310534787f07a04971722dbc9bb33b
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 2%

---

# [!DNL Verizon Media Group] （Yahoo、AOL、Verizon等）

[!DNL Verizon Media Group] 通常是大多数B2C列表的前三个域之一。 他们的行为有些独特，因为在出现声誉问题时，他们通常会限制或批量发送邮件。

以下是一些亮点：

## 什么数据重要

[!DNL Verizon Media Group] (VMG)使用内容和URL过滤以及垃圾邮件投诉的混合方式，构建并维护自己的专有垃圾邮件过滤器。 与Gmail一样，他们是早期采用的ISP之一，可以按域和IP地址过滤电子邮件。

## 他们提供哪些数据

VMG有一个FBL，用于将投诉信息反馈给发件人。 他们还在探索未来添加更多数据。

## 发件人声誉

发件人的声誉由IP地址、域和地址的组合组成。 声誉是使用传统组件计算的，这些组件包括投诉、垃圾邮件陷阱、不活动或格式错误的地址以及参与度。 VMG使用速率限制（也称为限制）和批量折叠来防御垃圾邮件。 它们用一些 [!DNL Spamhaus] 黑名单，包括PBL、SBL及XBL以保护其用户。

## 分析

VMG最近为旧的不活动电子邮件地址设置了定期维护期。 这意味着，通常会发现无效地址跳出次数大幅激增，这可能会在短时间内影响您的交付率。 发送者发送的无效地址退回率很高，这也表明需要收紧客户获取或参与政策。 发件人在大约1%的无效地址时，通常会遇到负面影响。
