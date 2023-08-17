---
title: Verizon Media Group（Yahoo、AOL、Verizon 等）
description: '"[!DNL Verizon Media Group] 通常是大多数B2C列表的前三个域之一。 他们的行为有些独特，因为如果出现信誉问题，他们通常会限制或发送大量邮件。”'
topics: Deliverability
jira: KT-5320
doc-type: article
activity: understand
role: Admin, Leader, User
level: Beginner
team: TM
exl-id: 43e6d3cb-23c3-4076-8026-a1a08e76bd1b
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 2%

---

# [!DNL Verizon Media Group] （Yahoo、AOL、Verizon等）

[!DNL Verizon Media Group] 通常是大多数B2C列表的前三个域之一。 他们的行为有些独特，因为如果出现信誉问题，他们通常会限制或批量发送邮件。

以下是一些要点：

## 哪些数据重要

[!DNL Verizon Media Group] (VMG)使用内容和URL过滤以及垃圾邮件投诉的组合，构建和维护了其自己的专有垃圾邮件过滤器。 与Gmail一样，它们也是早期采用的ISP之一，这些ISP按域和IP地址过滤电子邮件。

## 他们提供哪些数据

VMG有一个FBL，用于将投诉信息反馈给发件人。 他们还在探索将来添加更多数据。

## 发件人信誉

发件人的信誉由IP地址、域和发件人地址的组合组成。 信誉是使用传统组件计算的，包括投诉、垃圾邮件陷阱、不活动或格式错误的地址以及参与。 VMG使用速率限制（也称为限制）以及批量文件夹来抵御垃圾邮件。 它们用一些补充其内部过滤系统 [!DNL Spamhaus] 黑名单，包括PBL、SBL和XBL以保护其用户。

## 见解

VMG最近对旧的、非活动的电子邮件地址有定期维护期。 这意味着经常会看到无效地址退回量激增，这可能会在短时间内影响您的投放率。 它们还对发件人无效地址的高比率退回非常敏感，这表示需要收紧客户获取或参与政策。 发件人通常会在大约1%的无效地址遇到负面影响。
