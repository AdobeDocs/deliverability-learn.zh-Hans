---
title: Microsoft（Hotmail、Outlook、Windows Live 等）
description: 根據您清單的組成，Microsoft通常是第二大或第三大提供者，而且他們處理的流量與其他ISP略有不同。
topics: Deliverability
kt: 5319
doc-type: article
activity: understand
team: TM
exl-id: d706cb90-828a-4ab3-8f93-c9bd71553d63
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '334'
ht-degree: 2%

---

# [!DNL Microsoft] ([!DNL Hotmail]， [!DNL Outlook]， [!DNL Windows Live]、等)

[!DNL Microsoft] 根據您清單的組成，通常為第二大或第三大提供者，而且他們處理的流量與其他ISP略有不同。

以下是一些重點：

## 哪些資料重要

[!DNL Microsoft] 專注於寄件者信譽、投訴、使用者參與度以及他們自己輪詢意見的受信任使用者群組（也稱為寄件者信譽資料或SRD）。

## 他們提供哪些資料

[!DNL Microsoft]專屬的寄件者報告工具， [!DNL Smart Network Data Services] (SNDS)，可讓您檢視關於您傳送了多少郵件、接受了多少郵件，以及投訴和垃圾郵件陷阱的量度。 請記住，共用的資料為範例，不會反映確切數字，但最能代表如何進行 [!DNL Microsoft] 將您視為寄件者。 [!DNL Microsoft] 不會公開提供其受信任使用者群組的資訊，但可透過 [!DNL Return Path Certification] 計畫需額外付費。

## 寄件者信譽

[!DNL Microsoft] 傳統上專注於在信譽評估和篩選決定中傳送IP。 他們也在積極擴展傳送網域功能。 兩者主要受到投訴和垃圾郵件陷阱等傳統信譽影響者的推動。 「回訪路徑認證」計畫也會對傳遞能力產生重大影響，該計畫具有特定的定量和定性計畫要求。

## 见解

[!DNL Microsoft] 結合其所有接收網域，以建立及追蹤傳送信譽。 其中包括 [!DNL Hotmail]， [!DNL Outlook]， MSN， [!DNL Windows Live]，依此類推，以及任何企業Office 365代管電子郵件。 [!DNL Microsoft] 可能會對數量波動特別敏感，所以請考慮套用特定策略來增加或減少大型傳送的流量，而不是允許數量突然變更。

[!DNL Microsoft] 在IP暖機的最初幾天也特別嚴格，這通常意味著大多數郵件最初都會被過濾。 在證實有罪之前，大多數ISP會認為寄件者無罪。 [!DNL Microsoft] 恰恰相反，在您證明自己無罪之前，都會認為您有罪。
