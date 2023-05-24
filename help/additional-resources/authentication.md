---
title: 身份验证
description: 進一步瞭解SPF、DKIM和DMARC驗證方法。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 03609139-b39b-4051-bcde-9ac7c5358b87
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '762'
ht-degree: 0%

---

# 身份验证

## SPF {#spf}

SPF (Sender Policy Framework)是一種電子郵件驗證標準，允許網域的擁有者指定允許哪些電子郵件伺服器代表該網域傳送電子郵件。 此標準使用電子郵件的「Return-Path」標頭中的網域（也稱為「Envelope From」地址）。

>[!NOTE]
>
>您可以使用 [此外部工具](https://www.kitterman.com/spf/validate.html) 驗證SPF記錄。

SPF技術在一定程度上可讓您確保電子郵件中使用的網域名稱不是偽造的。 從網域收到訊息時，會查詢網域的DNS伺服器。 回應是簡短記錄（SPF記錄），詳細說明哪些伺服器有權從此網域傳送電子郵件。 如果我們假設只有網域擁有者才有辦法變更此記錄，我們可以認為此技術不允許偽造寄件者地址，至少不允許偽造「@」右側的部分。

在決賽中 [RFC 4408規格](https://www.rfc-editor.org/info/rfc4408)，會使用郵件的兩個元素來判斷視為寄件者的網域：SMTP 「HELO」（或「EHLO」）命令指定的網域，以及「Return-Path」（或「MAIL FROM」）標頭位址（也是退信地址）指定的網域。 不同的考量可讓您僅考量這些值之一；我們建議確定兩個來源都指定相同的網域。

檢查SPF會評估寄件者網域的有效性：

* **無**：無法執行評估。
* **中性**：查詢的網域未啟用評估。
* **通過**：網域視為正版。
* **失敗**：網域是偽造的，應該拒絕訊息。
* **SoftFail**：網域可能是偽造的，但不應僅根據此結果拒絕訊息。
* **TempError**：暫時錯誤停止評估。 可拒絕訊息。
* **PermError**：網域的SPF記錄無效。

值得一提的是，在DNS伺服器層級建立的記錄最多可能需要48小時的時間才能列入考量。 此延遲取決於接收伺服器的DNS快取重新整理的頻率。

## DKIM {#dkim}

DKIM (DomainKeys Identified Mail)驗證是SPF的後續驗證。 它使用公開金鑰加密技術，讓接收電子郵件伺服器可驗證訊息是否確實是由其聲稱傳送者所傳送的個人或實體所傳送，以及訊息內容在原始傳送時間（和DKIM「簽署」）與接收時間之間是否有變更。 此標準通常使用「寄件者」或「寄件者」標頭中的網域。

DKIM來自DomainKeys、Yahoo！ 和Cisco Identified Internet Mail驗證原則，用於檢查傳送者網域的真實性，並保證訊息的完整性。

已取代DKIM **網域索引鍵** 驗證。

使用DKIM需要一些先決條件：

* **安全性**：加密是DKIM的關鍵元素。 為確保DKIM的安全性等級，1024b是建議的最佳實務加密大小。 大多數存取提供者不會將較低的DKIM金鑰視為有效。
* **信譽**：信譽是以IP和/或網域為基礎，但不太透明的DKIM選擇器也是要考慮的關鍵元素。 選擇選擇器相當重要：請避免保留任何人都能使用的「預設」選擇器，以免信譽受損。 您必須實作另一個選擇器，用於 **保留與贏取通訊** 和用於驗證。

瞭解更多有關在中使用Campaign Classic時的DKIM先決條件 [本節](/help/additional-resources/acc-technical-recommendations.md#dkim-acc).

## DMARC {#dmarc}

DMARC （網域型訊息驗證、報告和符合性）是電子郵件驗證的最新形式，它仰賴SPF和DKIM驗證來判斷電子郵件通過還是失敗。 DMARC具有獨特而強大的功能，有兩個重要方面：

* 符合性 — 這可讓傳送者指示ISP如何處理任何無法驗證的訊息（例如：不接受）。
* 報告 — 它提供傳送者詳細的報告，顯示未通過DMARC驗證的所有郵件，以及每個郵件使用的「寄件者」網域和IP位址。 這可讓公司識別未通過驗證且需要某種「修正」型別的合法電子郵件（例如，將IP位址新增至其SPF記錄），以及其電子郵件網域上網路釣魚嘗試的來源和普遍程度。

>[!NOTE]
>
>DMARC可善用以下專案產生的報告： [250ok](https://250ok.com/).
