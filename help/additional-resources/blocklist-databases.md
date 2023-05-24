---
title: 实时黑洞列表
description: 瞭解維護垃圾郵件傳送者可能使用的IP位址和網域清單的組織。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4155b89f-a636-404c-8951-563c1b4d0289
source-git-commit: e7427d6109f3201affa58decde36294d1631bf5b
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 6%

---

# 实时黑洞列表

數個組織會維護據稱供垃圾郵件傳送者使用的IP位址和網域資料庫。 查閱這些網站有助於瞭解為什麼某些郵件會遭到拒絕成為垃圾郵件。 通常可以請求移除錯誤地新增到這些清單中的地址。

這些資料庫稱為RBL （即時黑洞清單），可透過DNS機制來查閱這些資料庫。 RBL有三種型別：

* 依IP位址：列出傳送垃圾郵件或可能轉送垃圾郵件的IP位址。
* 依寄件者網域：列出傳送垃圾郵件或設定錯誤的寄件者網域（退回郵件地址的完整網域）。
* 依網域：列出在垃圾郵件內容中所包含連結和影像的URL中找到的網域（向註冊機構註冊的高層網域）。 在Adobe解決方案中，要考量的網域通常是用於追蹤的地址。

以下列出最廣泛使用的RBL。 如需更完整的清單，請參閱 [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **Spamhaus**

   請參閱 [https://www.spamhaus.org/](https://www.spamhaus.org/)

   資料庫比較重要。 一般而言，被分類到這份清單中是很嚴重的情況。 如果發生這種情況，您必須立即採取行動並警告商業服務、傳遞能力和 [Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **垃圾訊息**

   請參閱 [https://www.spamcop.net/](https://www.spamcop.net/)

   它是最著名的資料庫之一。 如果您的其中一個IP位址放在這個清單中，這通常表示SpamCop使用者已將您的訊息宣告為Spam，或您已傳送訊息至SpamCop蜜罐。

* **URIBL**

   請參閱 [https://www.uribl.com/](https://www.uribl.com/)

   此清單可識別在宣告為垃圾訊息的訊息中經常出現的網域。 如果您的網域出現在此清單上，可能會大幅影響您的傳遞能力。 您應通知傳遞服務並 [Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **SURBL**

   請參閱 [https://surbl.org/](https://surbl.org/)

   SURBL會識別定期出現在垃圾郵件中的網站。 如果您的網域出現在此清單上，可能會大幅影響您的傳遞能力。 您應通知傳遞服務並 [Adobe客戶服務](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **iX馬尼圖**

   這是IP清單，在德國被廣泛使用。 請參閱 [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
