---
title: 重复
description: 瞭解如何識別並限制重複專案，以改善傳遞能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 6%

---

# 重复 {#duplicates}

擁有重複的電子郵件地址可能會有多重後果：

* 相同訊息已傳送多次。 即使Adobe在傳送前依預設會執行重複資料刪除程式，在分割目標時，也不會有內容相同的不同動作停止傳送相同的訊息。
* 未接受取消訂閱請求。 如果收件者在收到訊息後取消訂閱，其重複的設定檔仍符合未來訊息的資格。

除了這種選擇加入程式的側邊步驟外，這種情況可能會讓使用者將訊息視為垃圾郵件，並在ISP觸發封鎖清單程式。

在資料庫上執行操作時，您必須特別謹慎：

* 必須小心設定匯入，尤其是選擇調解金鑰時。
* 變更的電子郵件地址也可以是重複專案的來源。 舉例來說，如果公司變更名稱並維持先前網域特定時間： joe.doe@amce-co.com和joe.doe@acme-rebranded.com，會將網域不同的兩個位址路由至相同的信箱。
* 自動匯入（無論是清單或來自其他資料庫）是管理設定檔時要考慮的元素。 當您刪除或移動另一個分割中的設定檔時，會發生什麼情況？ 它可透過自動匯入在初始分割區中重新建立，例如，當下採購單時。
* 可以使用檢視而非分割來將設定檔儲存在不同的資料夾中。 如此一來，您就可以確定設定檔位於相同的實體分割區中，同時仍可顯示和管理足夠的許可權。

相同的情況下，不同分割區之間的重複專案是正常的。 例如，為第三方或不同公司實體傳送時，出於不同原因，同一人成為收件者符合邏輯。 然而，在相同分割區中尋找重複專案很少是正常的。

## 产品特定资源

對地址進行重複資料刪除可保護您的傳送信譽，並確保良好的隔離管理。 在以下產品檔案章節深入瞭解：

**Adobe Campaign Classic**

* [重複資料刪除活動](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用重複資料刪除活動的合併功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=zh-Hans)

**Adobe Campaign Standard**

* [从导入的文件中删除数据重复项](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
