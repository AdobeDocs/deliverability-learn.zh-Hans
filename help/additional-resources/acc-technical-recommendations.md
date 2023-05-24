---
title: Campaign Classic - 技术建议
description: 探索可用來透過Adobe Campaign Classic提高傳遞率的技術、設定和工具。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 39ed3773-18bf-4653-93b6-ffc64546406b
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '1575'
ht-degree: 1%

---

# Campaign Classic - 技术建议 {#technical-recommendations}

以下列出您在使用Adobe Campaign Classic時可用來改善傳遞率的幾項技術、設定和工具。

## 配置 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign會檢查是否已為IP位址指定反向DNS，而且這會正確指向IP。

網路設定中很重要的一點是要確保為傳出訊息的每個IP位址定義正確的反向DNS。 這表示對於指定的IP位址，會有反向DNS記錄（PTR記錄），且有相符的DNS （A記錄）回送至初始IP位址。

反向DNS的網域選擇在處理某些ISP時會產生影響。 尤其是AOL，它只接受在與反向DNS相同的網域中具有位址的回饋回圈(請參閱 [回饋迴路](#feedback-loop))。

>[!NOTE]
>
>您可以使用 [此外部工具](https://mxtoolbox.com/SuperTool.aspx) 驗證網域的設定。

### MX規則 {#mx-rules}

MX規則（郵件交換器）是管理傳送伺服器與接收伺服器之間通訊的規則。

更準確地說，它們用於控制Adobe Campaign MTA （訊息傳輸代理程式）傳送電子郵件給每個個別電子郵件網域或ISP (例如hotmail.com、comcast.net)的速度。 這些規則通常以ISP發佈的限製為基礎（例如，每個SMTP連線不包含超過20則訊息）。

>[!NOTE]
>
>如需Adobe Campaign Classic中MX管理的詳細資訊，請參閱 [本節](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration).

### TLS {#tls}

TLS （傳輸層安全性）是一種加密通訊協定，可用來保護兩個電子郵件伺服器之間的連線，並保護電子郵件內容不被預期的收件者以外的任何人讀取。

### 寄件者的網域 {#sender-domain}

若要定義用於HELO命令的領域，請編輯執行個體的組態檔(conf/config-instance.xml)並定義「localDomain」屬性，如下所示：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM網域是技術退信中所使用的網域。 此位址是在部署精靈中或透過NmsEmail_DefaultErrorAddr選項定義。

### SPF記錄 {#dns-configuration}

SPF記錄目前可在DNS伺服器上定義為TXT型別記錄（代碼16）或SPF型別記錄（代碼99）。 SPF記錄採用字元字串的形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

將兩個IP位址12.34.56.78和12.34.56.79定義為已授權可傳送網域的電子郵件。 **~all** 表示任何其他位址都應解譯為SoftFail。

用於定義SPF記錄的Recommendations：

* 新增 **~all** (SoftFail)或 **-all** （失敗）在結尾處拒絕除已定義伺服器以外的所有伺服器。 若無此專案，伺服器將能偽造此網域（使用中性評估）。
* 不要新增 **ptr** (openspf.org建議不要這麼做，因為這麼做成本高昂且不可靠)。

>[!NOTE]
>
>進一步瞭解SPF，請參閱 [本節](/help/additional-resources/authentication.md#spf).

## 身份验证

>[!NOTE]
>
>進一步瞭解中的不同電子郵件驗證形式 [本節](/help/additional-resources/authentication.md).

### DKIM {#dkim-acc}

>[!NOTE]
>
>對於託管或混合安裝，如果您已升級至 [增強型MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)，DKIM電子郵件驗證簽署是由Enhanced MTA針對所有網域的所有郵件完成。

使用 [DKIM](/help/additional-resources/authentication.md#dkim) 搭配Adobe Campaign Classic需要下列先決條件：

**Adobe Campaign選項宣告**：在Adobe Campaign中，DKIM私密金鑰是以DKIM選擇器和網域為基礎。 目前無法使用不同的選取器為相同的網域/子網域建立多個私密金鑰。 無法定義哪一個選取器網域/子網域必須用於平台或電子郵件中的驗證。 平台可選擇其中一個私密金鑰，這表示驗證很有可能失敗。

* 如果您已設定Adobe Campaign執行個體的DomainKeys，則只需選取 **dkim** 在 [網域管理規則](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). 如果沒有，請遵循與DomainKeys （取代DKIM）相同的設定步驟（私人/公開金鑰）。
* 不需要為相同的網域同時啟用DomainKeys和DKIM，因為DKIM是DomainKeys的改良版本。
* 下列網域目前驗證DKIM：AOL、Gmail。

## 回饋迴路 {#feedback-loop-acc}

回饋回圈的運作方式是在ISP層級針對用於傳送訊息的一系列IP位址宣告指定的電子郵件地址。 ISP會將收件者回報為垃圾訊息的訊息，以類似退回訊息的方式傳送至此信箱。 平台應設定為封鎖未來傳送給提出投訴的使用者。 即使他們未使用適當的退出連結，也不要再聯絡他們，這點很重要。 ISP會根據這些投訴將IP位址新增至封鎖清單。 根據ISP，約1%的投訴率將導致封鎖IP位址。

目前正在草擬一個標準，以定義回饋迴路訊息的格式： [濫用意見回饋報告格式(ARF)](https://tools.ietf.org/html/rfc6650).

實作例項的回饋回圈需要：

* 專用於執行個體的信箱，可能是退回信箱
* 專用於執行個體的IP傳送位址

在Adobe Campaign中實作簡單的回饋迴路時，會使用跳出訊息功能。 回饋回圈信箱已用作退回信箱，並已定義規則來偵測這些郵件。 會將回報訊息為垃圾訊息的收件者的電子郵件地址新增至隔離清單。

* 建立或修改退回郵件規則， **Feedback_loop**，在 **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** 包含原因 **已拒絕** 和型別 **硬**.
* 如果專為回饋回圈定義了信箱，請透過在中建立新的外部退回郵件帳戶來定義存取該信箱的引數 **[!UICONTROL Administration > Platform > External accounts]**.

此機制可立即運作，以處理申訴通知。 為確保此規則正常運作，您可以暫時停用帳戶，讓帳戶不收集這些郵件，然後手動檢查回饋回圈信箱的內容。 在伺服器上，執行下列命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果您被迫針對多個執行個體使用單一回饋回圈位址，您必須：

* 復寫在任意數目的信箱上收到的郵件，
* 讓每個信箱由單一執行個體擷取，
* 設定執行個體，使其僅處理與其相關的訊息：執行個體資訊包含在Adobe Campaign所傳送訊息的訊息ID標題中，因此也位於回饋回圈訊息中。 只需指定 **checkInstanceName** 執行個體設定檔案中的引數（依預設，執行個體不會驗證，這可能會導致某些位址被錯誤隔離）：

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign的傳遞服務可管理您對下列ISP的回饋回圈服務的訂閱：AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、Hotmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Telenor、Terra、UnitedOnline、USA、XS4ALL、Yahoo、Yahoo、Yahoo。

## 清單 — 取消訂閱 {#list-unsubscribe}

### 關於清單 — 取消訂閱 {#about-list-unsubscribe}

新增名為的SMTP標頭 **清單 — 取消訂閱** 是確保最佳化傳遞能力管理的必要專案。

此標頭可用作「回報為垃圾郵件」圖示的替代標頭。 它會在電子郵件介面中顯示為取消訂閱連結。

使用此功能有助於保護您的聲譽，意見反應將以取消訂閱的方式執行。

若要使用List-Unsubscribe，您必須輸入類似以下的命令列：

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>以上範例是根據收件者表格。 如果資料庫實作是從另一個表格完成的，請務必以正確的資訊重寫命令列。

下列命令列可用來建立動態 **清單 — 取消訂閱**：

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail、Outlook.com和Microsoft Outlook支援此方法，而且其介面中會直接提供取消訂閱按鈕。 此技巧可降低投訴率。

您可以實作 **清單 — 取消訂閱** 透過以下其中一種方式：

* 直接 [在傳遞範本中新增命令列](#adding-a-command-line-in-a-delivery-template)
* [创建分类规则](#creating-a-typology-rule)

### 在傳遞範本中新增命令列 {#adding-a-command-line-in-a-delivery-template}

必須在電子郵件的SMTP標頭的其他區段中新增命令列。

您可以在每封電子郵件或現有的傳遞範本中新增此內容。 您也可以建立包含此功能的新傳遞範本。

### 创建分类规则 {#creating-a-typology-rule}

規則必須包含產生命令列的指令碼，且必須包含在電子郵件標頭中。

>[!NOTE]
>
>建議您建立型別規則：List-Unsubscribe功能會自動新增至每封電子郵件。

1. 清單 — 取消訂閱： &lt;mailto:unsubscribe domain.com=&quot;&quot;>

   按一下 **取消訂閱** 連結會開啟使用者的預設電子郵件使用者端。 必須在用於建立電子郵件的型別中新增此型別規則。

1. 清單 — 取消訂閱： `<https://domain.com/unsubscribe.jsp>`

   按一下 **取消訂閱** 連結會將使用者重新導向至您的取消訂閱表單。

   示例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>瞭解如何在Adobe Campaign Classic中建立型別規則 [本節](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

## 電子郵件最佳化 {#email-optimization}

### SMTP {#smtp}

SMTP （簡易郵件傳輸通訊協定）是電子郵件傳輸的網際網路標準。

規則未檢查的SMTP錯誤會列在 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** 資料夾。 這些錯誤訊息預設會解譯為無法存取的軟錯誤。

必須找出最常見的錯誤，並在中新增對應的規則 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** 如果您希望正確地確認來自SMTP伺服器的意見回饋。 如果沒有此專案，平台將會執行不必要的重試（發生不明使用者的情況），或在特定數量的測試後，錯誤地將特定收件者置於隔離中。

### 專用IP {#dedicated-ips}

Adobe為每位客戶提供專屬的IP策略，並逐步提升IP，以建立聲譽並最佳化傳遞效能。
