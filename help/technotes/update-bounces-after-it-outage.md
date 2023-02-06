---
title: 在Italia Online中断后更新退件资格
description: 了解如何在Italia Online服务中断后更新退回资格
feature: Deliverability
source-git-commit: 4494363c060b7e2e1efde46f7ab8260fa900ffd7
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 0%

---

# 在意大利在线服务中断后更新错误的硬退回 {#update-bounce-italia}

## 上下文{#outage-context}

从1月22日（当地时间）开始，Italia Online发生了中断，导致了多次延迟和拒绝电子邮件。 服务于1月26日开始恢复，容量有限。

受影响的域包括： **libero.it**, **virgilio.it**, **inwind.it**, **iol.it**&#x200B;和 **blu.it**.

此问题发生在1/22/2023到1/26/2023之间，但大多数错误的隔离发生在1月26日。

在官方通信中了解详情 [此处](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}。


## 影响{#outage-impact}

与大多数ISP中断时一样，通过Campaign发送的某些电子邮件错误地标记为退回。 这不仅影响了Adobe，而且在停机期间，所有试图向Italia Online发送电子邮件的人。

症状为：

* **延期退回** 消息 `452 requested action aborted: try again later`  — 自动重试这些操作，无需执行任何操作。

* **硬退回** 消息 `550 <email address> recipient rejected` ISP已于当地时间1月26日早8点至晚2点返回，以防止发送方继续使其服务器过载。 正如意大利在线邮递员所确认的，这些地址并非真正的硬退回，因此我们建议取消对因该邮件而于2023年1月26日被排除的所有电子邮件地址的隔离。

## 更新流程{#outage-update}

根据标准退回处理逻辑，Adobe Campaign会通过 **[!UICONTROL Status]** 设置 **[!UICONTROL Quarantine]**. 要更正此问题，您需要通过查找和删除这些收件人，或更改其 **[!UICONTROL Status]** to **[!UICONTROL Valid]** 以便夜间清理工作流将删除它们。

要查找受此问题影响的收件人，或者如果在其他ISP中再次发生此问题，请参阅以下说明：

* 有关Campaign Classicv7和Campaign v8，请参阅 [本页](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 有关Campaign Standard，请参阅 [本页](https://experienceleague.corp.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。



