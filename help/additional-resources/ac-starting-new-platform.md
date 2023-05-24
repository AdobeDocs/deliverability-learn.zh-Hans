---
title: 启动新平台
description: 進一步瞭解使用Adobe Campaign啟動新平台時如何管理傳遞能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 6c9ade01-3052-4311-af80-888294820024
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 8%

---

# 启动新平台 {#starting-new-platform}

在設定要與Adobe Campaign搭配使用的新平台時，維護您的網域和IP位址信譽至關重要。

## 敏感步驟

在新平台上開始傳送電子郵件時，您應該非常小心，因為平台沒有任何使用記錄，而且從未將傳送IP用於此目的時，沒有信譽。

ISP自然會懷疑從未用來傳送電子郵件，且突然開始傳送大量電子郵件流量的IP位址。 事實上，垃圾郵件傳送者通常使用「未知」IP位址（從未列入封鎖清單的位址）在偵測之前傳送儘可能多的訊息。

在生產階段剛開始時，就輸出而言，您不可能指望達到作業速度。 此外，您不應嘗試以這個速率傳送訊息，因為這可能會導致ISP封鎖傳送位址，並嚴重危害啟動階段的其餘部分。

## 主要原則

以下列出啟動新平台時應遵循的主要原則。

* 設定專屬於從Adobe傳送之電子郵件行銷活動的專用子網域。

* 如果您有此資訊， **將無效的地址匯入隔離表格**.
首次使用位址清單且可能不完全合格時，經常會啟動平台。 如果您傳送至無效的位址或蜜罐位址，將降低平台的聲譽。

   * 如果您有無效地址清單，則在第一次傳送之前將其匯入隔離表格符合您的最大利益。 隔離表可透過以下網址取得： **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Non deliverables and addresses]** (Campaign Classic)和 **[!UICONTROL Administration > Channels > Quarantines > Addresses]** (Campaign Standard)功能表。

   * 儘管如此，如果您想要重新確認無效位址的資格，最好在平台的聲譽建立並逐位執行這項操作，以「稀釋」一段時間內使用錯誤位址。

* **限制傳輸率** 藉由限制配對的數量。 如需調整這類技術設定的詳細資訊，請聯絡您的Adobe Campaign管理員。

* **逐步增加傳送的磁碟區** 以避免被標籤為垃圾訊息。 不要從一開始就鎖定整個資料庫，而是在每次傳送時新增額外的清單部分。 這應該可讓您在減少無效位址的整體比率時，增加每個步驟的磁碟區。 若要確保啟動階段的順利開發，您可以使用波段。

* **定期傳送**. 在某種程度上，定期傳送小型快照比偶爾傳送大型行銷活動更好。
* **請密切注意傳遞報告**. 高錯誤指標可能表示技術設定設定設定錯誤。

## 其他资源

如需上述原則及其在Adobe Campaign實作方式的詳細資訊，請參閱下列章節：

* [利用 IP 预热提高您的电子邮件声誉](../../help/additional-resources/increase-reputation-with-ip-warming.md)
* [关于垃圾邮件陷阱](../../help/additional-resources/all-about-spam-traps.md)

**Adobe Campaign Classic**

* [透過隔離最佳化您的傳遞](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [識別整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-quarantine-management.html#identifying-quarantined-addresses-for-the-entire-platform)
* [使用多個波段傳送](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/key-steps-when-creating-a-delivery/steps-sending-the-delivery.html#sending-using-multiple-waves)
* [投放监测](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/about-delivery-monitoring.html?lang=zh-Hans#sending-messages)

**Adobe Campaign Standard**

* [透過隔離最佳化您的傳遞](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html#optimizing-your-delivery-through-quarantines)
* [識別整個平台的隔離地址](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/understanding-quarantine-management.html)
* [监测投放](https://experienceleague.adobe.com/docs/campaign-standard/using/testing-and-sending/monitoring-messages/monitoring-a-delivery.html?lang=zh-Hans)
