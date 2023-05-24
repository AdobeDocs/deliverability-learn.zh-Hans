---
title: 在Italia Online中斷後更新彈回資格
description: 瞭解如何在Italia Online中斷後更新彈回資格
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

自1月22日（當地時間）起，Italia Online已發生中斷，導致數次延遲並拒絕電子郵件。 服務於1月26日以有限容量開始恢復。

受影響的網域包括： **libero.it**， **virgilio.it**， **inwind.it**， **iol.it**、和 **blu.it**.

此問題發生在2023年1月22日到2023年1月26日，但大多數錯誤隔離發生在1月26日。

在官方通訊中瞭解更多 [此處](https://tecnologia.libero.it/avviato-il-ritorno-online-di-libero-mail-e-virgilio-mail-66832){_blank}。


## 影响{#outage-impact}

如同大多數網際網路服務提供者(ISP)發生中斷的情況一樣，透過Campaign或Journey Optimizer傳送的某些電子郵件被錯誤標籤為跳出。 這不僅會影響Adobe，也會影響所有嘗試在中斷期間將電子郵件傳送到Italia Online的人。

症狀如下：

* **軟退信** 包含訊息 `452 requested action aborted: try again later`  — 會自動重試，且不需要採取任何動作。

* **硬跳出** 包含訊息 `550 <email address> recipient rejected` ISP已於1月26日當地時間上午8點至下午2點之間傳回，以防止寄件者持續讓伺服器超載。 如Italia線上郵局主管所確認，這些並不是真正的硬跳出，因此我們建議取消隔離2023年1月26日因該訊息而被排除的所有電子郵件地址。

## 更新流程{#outage-update}

### Adobe Campaign{#ac-update}

根據標準退信處理邏輯，Adobe Campaign會使用自動將這些收件者新增至隔離清單 **[!UICONTROL Status]** 設定 **[!UICONTROL Quarantine]**. 若要修正此問題，您需要尋找並移除這些收件者，或變更其收件者，以更新Campaign中的隔離表格 **[!UICONTROL Status]** 至 **[!UICONTROL Valid]** 以便「夜間清理」工作流程會將其移除。

若要尋找受此問題影響的收件者，或當此問題再次發生在任何其他ISP上時，請參閱下列指示：

* 如需Campaign Classicv7和Campaign v8的相關資訊，請參閱 [此頁面](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。
* 如需Campaign Standard，請參閱 [此頁面](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html?lang=en#unquarantine-bulk){_blank}。

### Adobe Journey Optimizer{#ajo-update}

根據標準退信處理邏輯，Adobe Journey Optimizer會使用自動將這些電子郵件地址新增到隱藏清單 **[!UICONTROL Reason]** 設定 **[!UICONTROL Invalid Recipient]**. 若要修正此問題，您需要透過尋找並移除這些電子郵件地址來更新隱藏清單。

識別地址後，您可使用手動從隱藏清單中移除這些地址 **[!UICONTROL Delete]** 按鈕。 這些地址隨後可包含在未來的電子郵件行銷活動中。

進一步瞭解 [本節](https://experienceleague.adobe.com/docs/journey-optimizer/using/configuration/monitor-reputation/manage-suppression-list.html#remove-from-suppression-list){_blank}。

