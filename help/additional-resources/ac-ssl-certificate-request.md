---
title: SSL憑證請求流程
description: 瞭解如何在委派給Adobe的子網域上安裝SSL憑證。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 57016f89df54d5c74755a6a108a92db45153ec18
workflow-type: tm+mt
source-wordcount: '2252'
ht-degree: 3%

---

# SSL 证书请求进程

將傳送電子郵件的網域委派給Adobe後(請參閱 [網域名稱設定](/help/additional-resources/ac-domain-name-setup.md))，Adobe會針對特定功能建立和使用特定子網域。

例如，如果您已委派 *email.example.com* 若要Adobe傳送電子郵件，Adobe會建立下列子網域：
* *t.email.example.com*  — 用於追蹤連結
* *m.email.example.com*  — 適用於映象頁面
* *res.email.example.com*  — 用於託管資源（例如影像）

建議用於 **透過SSL (HTTPS)保護這些網域**. 事實上，不安全的連結(HTTP)很容易遭到攔截，並會在現代瀏覽器上標示警告。

若要在這些子網域上安裝SSL憑證，此程式包括請求CSR檔案，然後購買SSL憑證以供Adobe安裝或續約。

>[!CAUTION]
>
>在安裝SSL憑證之前，請確定您知道上列出的先決條件 [此頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=zh-Hans#installing-ssl-certificate).
>
>Adobe僅支援最多2048位元憑證。 尚未支援4096位元憑證。

## 术语表

| 搜索词 | 描述 |
|--- |--- |
| CA （憑證授權單位） | SSL憑證提供者，可在驗證組織或個人（例如DigiCert、Symantec等）的身分後，向他們發行數位憑證。<ul><li>受信任的CA通常被認為是發行根憑證的第三方CA。</li><li>如果憑證是由使用該憑證的相同組織/公司簽署，即使它們是SSL憑證（例如自我簽署憑證），也會分類為不受信任的CA。</li></ul> |
| 鏈結憑證 | 包含根憑證和一個或多個中間憑證的憑證稱為鏈結（或連結）憑證。 |
| CSR （憑證申請檔） | 申請SSL憑證時，為憑證授權單位提供的編碼文字區塊。 它通常會在安裝憑證的伺服器上產生。 |
| DER （辨別編碼規則） | 憑證延伸型別。 .der副檔名用於二進位DER編碼的憑證。 這些檔案也可能支援.cer或.crt副檔名。 |
| EV （延伸驗證）憑證 | EV憑證是專為防止網路釣魚攻擊而設計的新憑證型別。 這需要對您的企業和訂購憑證的人員進行延伸驗證。 |
| 高保證憑證 | CA在驗證網域名稱的所有權和有效的企業註冊後，會核發高保證憑證。 |
| 中繼CA | 包含在鏈結憑證中的中間憑證的憑證授權單位。 |
| 中繼憑證 | 憑證授權單位會以樹狀結構的形式發行憑證。 根憑證是樹狀結構的最上層憑證。 憑證和根憑證之間的任何憑證都稱為鏈結或中繼憑證。 |
| 低保證憑證 | 低保證憑證（也稱為網域驗證憑證）僅包括憑證中的網域名稱（不包括企業/組織名稱）。 |
| PEM （隱私權增強郵件） | 副檔名為.pem的憑證，其中包含ASCII (Base64)資料。 這類憑證以「 — — — 開始憑證 — — - 」行開頭。 |
| 根憑證 | 憑證授權單位會以樹狀結構的形式發行憑證。 根憑證是樹狀結構的最上層憑證。 |
| SAN （主體替代名稱） | 主旨的替代名稱是其他主機名稱（網站、IP位址、通用名稱等） 應該簽署為單一SSL憑證的一部分。 |
| 自我簽署憑證 | 由建立憑證的人簽署的憑證，而不是受信任的憑證授權單位。 自我簽署憑證可以啟用與CA簽署的憑證相同的加密層級，但有兩個主要缺點：<ul><li>訪客的連線可能會被劫持，使得攻擊者能夠檢視所有傳送的資料（因而降低了加密連線的目的）</li><li> 無法像受信任的憑證一樣撤銷憑證。</li></ul> |
| SSL （安全通訊端層） | 在網頁伺服器和瀏覽器之間建立加密連結的標準安全性技術。 |
| 萬用字元憑證 | 萬用字元憑證可保護單一網域名稱(例如*.adobe.com)上不限數量的一級子網域。 |

## 主要步驟

1. 要求Certificate Signing Request (CSR)檔案，並提供必要資訊（國家/地區、州、城市、組織名稱、組織單位名稱等） 以Adobe。
1. 驗證Adobe產生的CSR檔案，並驗證您提供的所有資訊是否正確。
1. 使用CSR詳細資料來產生由受信任的憑證授權單位簽署的憑證<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->.
1. 驗證SSL憑證，並確認其符合CSR。
1. 向Adobe提供SSL憑證，由誰進行安裝。
1. 測試是否已成功為每個安全子網域安裝SSL憑證。
1. 監視SSL憑證有效期。
1. 更新Adobe Campaign中的任何特定設定。

## 詳細程式

### 先决条件

您必須識別網域名稱和功能（追蹤、映象頁面、網頁應用程式等） 以保全。
>[!NOTE]
>
>Adobe有助於定義要涉及的網域名稱和函式。 如需詳細資訊，請聯絡您的Adobe客戶團隊。

### 步驟1 — 取得CSR檔案

若要取得CSR （憑證申請檔）檔案，請遵循下列步驟。

* 如果您有權存取 [控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請依照以下說明操作： [此頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#subdomains-and-certificates) 從「控制面板」產生並下載CSR檔案。

* 否則，請透過https://adminconsole.adobe.com/建立支援票證，以從Adobe客戶服務取得所需子網域的CSR檔案。

以下是一些要遵循的最佳實務：

* 針對每個委派的子網域引發一個請求。
* 您可以將多個子網域合併為單一CSR請求，但僅限於相同環境中。 例如，在Campaign Classic中，行銷伺服器、 [中間來源伺服器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html)，以及 [執行例項](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance) 有三個不同的環境。
* 在續約SSL憑證之前，您必須先取得新的CSR。 請勿使用一年或更久以前的舊CSR檔案。

您需要提供下列資訊。

>[!CAUTION]
>
>必須填寫下表中指示的所有欄位。 否則，無法處理CSR請求。

**要透過Adobe團隊協助提供的資訊：**

| 要提供的資訊 | 範例值 | 注意 |
|--- |--- |--- |
| 客户端名称 | 我的公司 | 您的組織名稱。 Adobe會使用此欄位來追蹤您的請求（它不會成為CSR/SSL憑證的一部分）。 |
| Adobe Campaign環境URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign執行個體URL。 |
| 一般名稱 [CN] | t.subdomain.customer.com | 這可以是任何相關網域，但通常是追蹤網域。 |
| 主體替代名稱 [SAN] | t.subdomain.customer.com | 請務必將追蹤子網域加入為SAN。 |
| 主體替代名稱 [SAN] | m.subdomain.customer.com |
| 主體替代名稱 [SAN] | res.subdomain.customer.com |

**您的IT/SSL內部團隊要提供的資訊：**

| 要提供的資訊 | 範例值 | 注意 |
|--- |--- |--- |
| 國家 [C] | US | 這必須是雙字母代碼。 存取完整的國家/地區清單 [此處](https://www.ssl.com/csrs/country_codes/).</br>*注意：若為英國，請使用GB （而非英國）。* |
| 州/省（或省名） [ST] | 伊利諾州 | 若適用。 值必須是完整名稱，而非縮寫。 |
| 城市/地區名稱 [L] | 芝加哥 |
| 組織名稱 [O] | ACME |
| 組織單位名稱 [OU] | IT |

>[!NOTE]
>
>將「subdomain.customer.com」取代為您委派的子網域，並將其他範例值取代為適當的值。

### 步驟2 — 驗證CSR檔案

在提交您的要求與相關資訊後，Adobe會產生並提供您憑證簽署要求(CSR)檔案。

產生的CSR檔案中的文字開頭必須是 **&quot;-----開始憑證要求-----&quot;**.

從Adobe收到CSR檔案後，請遵循以下步驟：

1. 將CSR檔案文字複製並貼到線上解碼器中，例如https://www.sslshopper.com/csr-decoder.html， <!--https://www.certlogik.com/decoder/,--> 或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您可以使用 *OpenSSL* 在Linux機器上本機執行命令。
1. 確認所有檢查均成功。
1. 檢查是否包含正確的引數和網域名稱。
1. 確認所有其他資料均符合您在提交請求時提供的詳細資料。

### 步驟3 — 產生SSL憑證

提供CSR檔案後，您必須使用CSR檔案為適當的網域購買並產生SSL憑證。

* SSL憑證：
   * 必須為Apache PEM格式；
   * 長度不應超過2048位元；
   * 必須由有效的CA （憑證授權單位）簽署；
   * 必須包括CSR檔案中提到的所有SAN （主體替代名稱）。
* 如果有一個或多個中間憑證，您必須提供根憑證和所有要Adobe的中間憑證。
* 您可以設定任何憑證有效期，但Adobe建議選擇足夠長的時間（例如兩年）。

>[!NOTE]
>
>如果您使用自己的內部工具或CA提供的入口網站來要求憑證，請務必使用CSR要求中提供的相同詳細資訊，以避免憑證產生程式的任何延遲或差異。

### 步驟4 — 驗證SSL憑證

產生SSL憑證後，您必須先驗證該憑證，才能將其傳送至Adobe。 要实现此目的，请执行以下步骤：

1. 請確定憑證的副檔名為.pem。 如果不是這種情況，請將其轉換為PEM格式。 您可以使用以下專案來進行轉換 *OpenSSL*.
1. 確認憑證的開頭為 **&quot;-----開始憑證-----&quot;**.
1. 將憑證文字複製到線上解碼器中，例如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您可以使用 *OpenSSL* 在Linux機器上本機執行命令。 有關詳細資訊，請參閱 [此外部頁面](https://www.shellhacks.com/decode-ssl-certificate/).
1. 請確定憑證正確解析，包括一般名稱、SAN、簽發者和有效期。
1. 如果SSL憑證驗證成功，請使用檢查憑證是否符合的CSR [此網站](https://www.sslshopper.com/certificate-key-matcher.html)：選取 **檢查CSR和憑證是否相符**，並在對應的欄位中輸入憑證和CSR。 兩者應相符。

### 步驟5 — 要求SSL憑證安裝

* 如果您有權存取 [控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，請依照以下說明操作： [此頁面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=zh-Hans#installing-ssl-certificate) 將憑證上傳至「控制面板」。

* 否則，請透過https://adminconsole.adobe.com/建立另一個支援票證，以要求Adobe在Adobe伺服器上安裝憑證。

您必須提供：

* 憑證檔案、根憑證和任何中間憑證（附加到票證），最好是Apache PEM格式。
* 為CSR引發的上一個支援票證編號。
* 與CSR票證提供的資料相同（包括通用名稱、執行個體URL、州、城市/地區、組織名稱、組織單位名稱等）。

### 步驟6 — 測試SSL憑證安裝

安裝SSL憑證並由Adobe客戶服務確認後，請確定已為所有URL成功安裝SSL憑證。

在關閉SSL安裝票證之前，請執行以下測試。 另請確定您依照「 」中的指示更新任何特定設定 [本節](#update-configuration).

導覽至瀏覽器中的下列URL （將「subdomain.customer.com」取代為您的子網域）：

* https://subdomain.customer.com/r/test (適用於 [網頁應用程式](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html) 僅限子網域 — 不適用於電子郵件子網域)
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的結果會提供環境資訊，而URL中的位址列表示連線是安全的。 例如，您會在Google Chrome中看到下列訊息：

![](../../help/assets/ssl-google-successful-result.png)

如果SSL憑證未正確安裝，則會顯示下列警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步驟7 — 檢查憑證有效期

您可以在瀏覽器中檢查憑證的有效期。 例如，在Google Chrome中，按一下 **安全** > **憑證**.

檢查有效期間是您的責任。 Adobe建議您實作程式來監視憑證到期。 進一步瞭解SSL憑證過期時會發生什麼情況 [本文](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/).

* 建立支援票證，以便在憑證到期日前至少兩週要求更新憑證。 除非CSR詳細資料已變更，否則您不需要請求其他CSR。

* 如果您有權存取 [控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，如果您的環境是由AWS環境中的Adobe託管，您可以使用「控制面板」在憑證過期前續約憑證。 有关详细信息，请参阅[此部分](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates)。

### 步驟8 — 更新任何特定設定 {#update-configuration}

在您確定已正確安裝請求的SSL憑證後，就可以將Adobe Campaign中的所有參考從HTTP更新為HTTPS。

>[!NOTE]
>
>若為Campaign Classic，要更新的URL主要位於 [部署精靈](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard) 和 [外部帳戶](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html) （追蹤、映象頁面和公共資源網域）。 如需Campaign Standard，請參閱 [品牌設定](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity).

更新設定後，新電子郵件將使用HTTPS URL而不是HTTP傳送。 若要檢查URL現在是否安全，您可以快速執行下列測試：

* 從Adobe Campaign上傳影像。 上傳影像後，傳回的URL應該是HTTPS。
* 建立測試電子郵件傳遞，包括映象頁面連結、部分影像、文字和取消訂閱連結。 將電子郵件傳送至外部電子郵件ID （例如您的Gmail地址）。 收到電子郵件後，請開啟電子郵件，並確認電子郵件內的所有連結都以其HTTPS表單（而非HTTP）正確開啟，不會出現任何SSL憑證警告或錯誤。

## 产品特定资源

**Campaign Classic**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 瞭解如何新增SSL憑證以保護您的子網域。

**Campaign Standard**

* [控制面板：新增SSL憑證（教學課程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html?lang=zh-Hans)  — 瞭解如何新增SSL憑證以保護您的子網域。
