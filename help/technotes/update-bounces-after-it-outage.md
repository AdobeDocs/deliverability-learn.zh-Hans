---
title: 在意大利在线中断后更新退回限定条件
description: 了解如何在意大利在线中断后更新退回鉴别
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
role: Admin
level: Beginner
TQID: 'https://experienceleague.adobe.com/hPHB9s3PH7E9L3omZMTWZA2ZB0d1JS5vEfawQOjnBLw'
product_v2:
  - id: b27e5950-9033-45ac-9f86-eb22e567f615
    internal-label: Marketo Engage
  - id: d0a3eab4-7b10-4d96-a71e-6c0f8e7b7c87
    internal-label: CX Enterprise
  - id: dfc56824-e8b9-499e-85d4-21aedb507314
    internal-label: Campaign
feature_v2:
  - id: a075b2c1-7748-4328-b7f6-343aa314616a
    internal-label: Campaigns
  - id: b3b8a63f-51fc-40f6-a7d2-a31c5d49fb45
    internal-label: Configuration
  - id: f71e690b-4480-4b67-9ef5-88f42f9cdfdb
    internal-label: Resources
  - id: 63876777-85c3-57e1-a2da-81f02956c63c
    internal-label: Deliverability
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: d66463b8c8097e19fd50b9b033e6b21165d7e8c9
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 4%
---
# 在意大利在线服务中断后更新错误的硬退回 {#update-bounce-italia}

## 上下文{#outage-context}

从1月22日（当地时间）开始，Italia Online经历了多次中断，导致多次延迟并拒绝电子邮件。 1月26日，该服务在有限容量下开始恢复。

受影响的域包括：**libero.it**、**virgilio.it**、**inwind.it**、**iol.it**&#x200B;和&#x200B;**blu.it**。

此问题发生在2023年1月22日至2023年1月26日，但大多数错误隔离发生在1月26日。

在官方沟通中[此处](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}了解详情。


## 影响{#outage-impact}

与大多数互联网服务提供商(ISP)发生中断的情况一样，通过Campaign或Journey Optimizer发送的一些电子邮件被错误地标记为跳出。 这不仅影响了Adobe，还影响了在服务中断期间尝试将电子邮件发送到意大利在线的每一个人。

症状为：

* **软退回**，消息为`452 requested action aborted: try again later` — 已自动重试这些退回，无需任何操作。

* ISP已于1月26日当地时间上午8点到下午2点之间返回带有消息`550 <email address> recipient rejected`的&#x200B;**硬退件**，以防止发件人不断超出其服务器。 正如意大利在线邮局主管所确认的，这些都不是真正的硬退件，因此我们建议取消隔离2023年1月26日因该消息而被排除的所有电子邮件地址。

## 更新流程{#outage-update}

### Adobe Campaign{#ac-update}

根据标准退回处理逻辑，Adobe Campaign已使用&#x200B;**[!UICONTROL Status]**&#x200B;设置&#x200B;**[!UICONTROL Quarantine]**&#x200B;将这些收件人自动添加到隔离列表。 要更正此问题，您需要在Campaign中更新隔离表，方法是查找并移除这些收件人，或将其&#x200B;**[!UICONTROL Status]**&#x200B;更改为&#x200B;**[!UICONTROL Valid]**，以便夜间清理工作流将移除这些收件人。

要查找受此问题影响的收件人，或在其他任何ISP再次出现此问题的情况下，请参阅以下说明：

* 对于Campaign Classic v7和Campaign v8，请参阅[此页面](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 对于Campaign Standard，请参阅[此页面](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根据标准退回处理逻辑，Adobe Journey Optimizer已使用&#x200B;**[!UICONTROL Reason]**&#x200B;设置&#x200B;**[!UICONTROL Invalid Recipient]**&#x200B;将这些电子邮件地址自动添加到禁止列表。 要更正此问题，您需要通过查找并删除这些电子邮件地址来更新禁止显示列表。

识别地址后，可以使用&#x200B;**[!UICONTROL Delete]**&#x200B;按钮从禁止显示列表中手动删除这些地址。 这些地址随后可以包含在将来的电子邮件营销活动中。

有关详细信息，请参阅[此部分](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}。

