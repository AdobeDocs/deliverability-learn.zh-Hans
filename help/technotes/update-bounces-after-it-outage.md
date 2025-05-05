---
title: 在意大利在线中断后更新退回限定条件
description: 了解如何在意大利在线中断后更新退回鉴别
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
role: Admin
level: Beginner
source-git-commit: 6b312cdbba496818337c97ec4f42962aea757901
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 3%

---

# 在意大利在线服务中断后更新错误的硬退回 {#update-bounce-italia}

## 上下文{#outage-context}

从1月22日（当地时间）开始，Italia Online经历了多次中断，导致多次延迟并拒绝电子邮件。 1月26日，该服务在有限容量下开始恢复。

受影响的域包括：**libero.it**、**virgilio.it**、**inwind.it**、**iol.it**&#x200B;和&#x200B;**blu.it**。

此问题发生在2023年1月22日至2023年1月26日，但大多数错误隔离发生在1月26日。

在官方沟通中[此处](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}了解详情。


## 影响{#outage-impact}

与大多数互联网服务提供商(ISP)发生中断的情况一样，通过Campaign或Journey Optimizer发送的一些电子邮件被错误地标记为跳出。 这不仅影响到Adobe，而且还会影响所有试图在服务中断期间将电子邮件发送到意大利在线的用户。

症状为：

* **软退回**，消息为`452 requested action aborted: try again later` — 已自动重试这些退回，无需任何操作。

* ISP已于1月26日当地时间上午8点到下午2点之间返回带有消息`550 <email address> recipient rejected`的&#x200B;**硬退件**，以防止发件人不断超出其服务器。 正如意大利在线邮局主管所确认的，这些都不是真正的硬退件，因此我们建议取消隔离2023年1月26日因该消息而被排除的所有电子邮件地址。

## 更新流程{#outage-update}

### Adobe Campaign{#ac-update}

根据标准退回处理逻辑，Adobe Campaign已使用&#x200B;**[!UICONTROL Status]**&#x200B;设置&#x200B;**[!UICONTROL Quarantine]**&#x200B;将这些收件人自动添加到隔离列表。 要更正此问题，您需要在Campaign中更新隔离表，方法是查找并移除这些收件人，或将其&#x200B;**[!UICONTROL Status]**&#x200B;更改为&#x200B;**[!UICONTROL Valid]**，以便夜间清理工作流将移除这些收件人。

要查找受此问题影响的收件人，或在其他任何ISP再次出现此问题的情况下，请参阅以下说明：

* 有关Campaign Classicv7和Campaign v8的信息，请参阅[此页面](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=zh-Hans#unquarantine-bulk){_blank}。
* 有关Campaign Standard，请参阅[此页面](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=zh-Hans#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根据标准退回处理逻辑，Adobe Journey Optimizer已使用&#x200B;**[!UICONTROL Reason]**&#x200B;设置&#x200B;**[!UICONTROL Invalid Recipient]**&#x200B;将这些电子邮件地址自动添加到禁止列表。 要更正此问题，您需要通过查找并删除这些电子邮件地址来更新禁止显示列表。

识别地址后，可以使用&#x200B;**[!UICONTROL Delete]**&#x200B;按钮从禁止显示列表中手动删除这些地址。 这些地址随后可以包含在将来的电子邮件营销活动中。

在[本节](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html?lang=zh-Hans#remove-from-suppression-list){_blank}中了解详情。

