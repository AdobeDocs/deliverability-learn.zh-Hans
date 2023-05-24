---
title: 在意大利在线中断后更新退回限定条件
description: 了解如何在Italia在线中断后更新退回鉴别
feature: Deliverability
exl-id: a11e88cf-bf37-42cc-9c09-1d58360459b7
hide: true
hidefromtoc: true
source-git-commit: aca77fb9326e34455a6fec7ffc9a7ad8e1750467
workflow-type: tm+mt
source-wordcount: '422'
ht-degree: 3%

---

# 在意大利在线服务中断后更新错误的硬退回 {#update-bounce-italia}

## 上下文{#outage-context}

从1月22日（当地时间）开始，Italia Online经历了多次中断，导致多次延迟并拒绝电子邮件。 1月26日，该服务在有限容量下开始恢复。

受影响的域包括： **libero.it**， **virgilio.it**， **inwind.it**， **iol.it**、和 **blu.it**.

此问题发生在2023年1月22日至2023年1月26日，但大多数错误隔离发生在1月26日。

在官方沟通中了解详情 [此处](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}。


## 影响{#outage-impact}

与大多数互联网服务提供商(ISP)发生中断的情况一样，通过Campaign或Journey Optimizer发送的一些电子邮件被错误地标记为跳出。 这不仅影响到Adobe，而且还会影响在服务中断期间尝试将电子邮件发送到Italia Online的所有人。

症状如下：

* **软退回** 包含消息 `452 requested action aborted: try again later`  — 这些操作会自动重试，无需执行任何操作。

* **硬退回** 包含消息 `550 <email address> recipient rejected` ISP已于1月26日当地时间上午8点至下午2点之间返回，以防止发件人不断超出其服务器。 正如意大利在线邮局主管所确认的，这些都不是真正的硬退回，因此我们建议取消隔离2023年1月26日因该消息而被排除的所有电子邮件地址。

## 更新流程{#outage-update}

### Adobe Campaign{#ac-update}

根据标准退回处理逻辑，Adobe Campaign会使用自动将这些收件人添加到隔离列表 **[!UICONTROL Status]** 设置 **[!UICONTROL Quarantine]**. 要更正此问题，您需要通过查找并移除这些收件人或更改其在Campaign中的隔离表来更新隔离表 **[!UICONTROL Status]** 到 **[!UICONTROL Valid]** 以便“夜间清理”工作流会删除它们。

要查找受此问题影响的收件人，或者如果任何其他ISP再次发生此情况，请参阅以下说明：

* 有关Campaign Classicv7和Campaign v8的信息，请参阅 [此页面](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 有关Campaign Standard，请参阅 [此页面](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根据标准退回处理逻辑，Adobe Journey Optimizer会使用自动将这些电子邮件地址添加到禁止显示列表 **[!UICONTROL Reason]** 设置 **[!UICONTROL Invalid Recipient]**. 要更正此问题，您需要通过查找并删除这些电子邮件地址来更新禁止显示列表。

标识地址后，可以使用手动从禁止显示列表中删除这些地址 **[!UICONTROL Delete]** 按钮。 然后，这些地址可以包含在将来的电子邮件营销活动中。

了解详情，请参阅 [本节](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}。

