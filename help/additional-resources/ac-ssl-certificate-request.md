---
title: SSL证书请求进程
description: 了解如何在委派给Adobe的子域上安装SSL证书。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 57016f89df54d5c74755a6a108a92db45153ec18
workflow-type: tm+mt
source-wordcount: '2124'
ht-degree: 1%

---

# SSL 证书请求进程

将域委派给Adobe以发送电子邮件后（请参阅[域名设置](/help/additional-resources/ac-domain-name-setup.md)），Adobe将创建并使用特定子域执行特定功能。

例如，如果您已委派&#x200B;*email.example.com*&#x200B;来Adobe发送电子邮件，则Adobe将创建如下所示的子域：
* *t.email.example.com* — 用于跟踪链接
* *m.email.example.com* — 用于镜像页面
* *res.email.example.com* — 用于托管资源（如图像）

建议通过SSL (HTTPS)****&#x200B;保护这些域。 事实上，不安全的链接(HTTP)很容易被拦截，并且会在现代浏览器上标出警告。

要在这些子域上安装SSL证书，此过程包括请求CSR文件，然后为Adobe购买SSL证书以进行安装或续订。

>[!CAUTION]
>
>在安装SSL证书之前，请确保您了解[此页面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=zh-Hans#installing-ssl-certificate)上列出的先决条件。
>
>Adobe最多仅支持2048位证书。 尚不支持4096位证书。

## 术语表

| 搜索词 | 描述 |
|--- |--- |
| CA（证书颁发机构） | SSL证书提供商，在验证组织或个人（如DigiCert、Symantec等）的标识后向其颁发数字证书。<ul><li>受信任的CA通常被视为颁发根证书的第三方CA。</li><li>如果证书由使用该证书的相同组织/公司签名，则即使它们是SSL证书（如自签名证书），该证书也会被分类为不受信任的CA。</li></ul> |
| 链证书 | 包含根证书和一个或多个中间证书的证书称为链（或链接）证书。 |
| CSR（证书签名请求） | 申请SSL证书时提供给证书颁发机构的编码文本块。 它通常在安装证书的服务器上生成。 |
| DER（可分辨编码规则） | 证书扩展类型。 .der扩展用于二进制DER编码证书。 这些文件可能还支持.cer或.crt扩展名。 |
| EV（扩展验证）证书 | EV证书是一种新型证书，旨在防止网络钓鱼攻击。 它要求对贵机构的业务和订购证书的人员进行扩展验证。 |
| 高保证证书 | 高保证证书在验证域名的所有权和有效的商业注册后由CA颁发。 |
| 中间CA | 链证书中包含的中间证书的证书颁发机构。 |
| 中间证书 | 证书颁发机构以树结构的形式颁发证书。 根证书是树的最顶部证书。 证书和根证书之间的任何证书称为链或中间证书。 |
| 低保证证书 | 低保证证书（也称为域验证证书）仅包括证书中的域名（不包括业务/组织名称）。 |
| PEM（隐私增强邮件） | 扩展名为.pem的证书，包含ASCII (Base64)数据。 此类证书以“ — — — 开始证书 — — - ”行开头。 |
| 根证书 | 证书颁发机构以树结构的形式颁发证书。 根证书是树的最顶部证书。 |
| SAN （主题替代名称） | 主题备用名称是其他主机名（站点、IP地址、通用名称等） 这些证书应该作为单个SSL证书的一部分签名。 |
| 自签名证书 | 由创建证书的人而不是受信任的证书颁发机构签名的证书。 自签名证书可以启用与CA签名的证书相同级别的加密，但有两个主要缺点：<ul><li>访客的连接可能被劫持，使得攻击者能够查看发送的所有数据（从而破坏加密连接的目的）</li><li> 证书无法像受信任的证书那样被吊销。</li></ul> |
| SSL（安全套接字层） | 用于在Web服务器和浏览器之间建立加密链接的标准安全技术。 |
| 通配符证书 | 通配符证书可以保护单个域名(例如*.adobe.com)上无限数量的第一级子域。 |

## 主要步骤

1. 索取证书签名请求(CSR)文件，并提供所需信息（国家/地区、州、城市、组织名称、组织单位名称等） Adobe。
1. 验证由Adobe生成的CSR文件，并验证您提供的所有信息是否正确。
1. 使用CSR详细信息生成由受信任的证书颁发机构<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->签名的证书。
1. 验证SSL证书并验证它是否与CSR匹配。
1. 向Adobe提供SSL证书，由他们进行安装。
1. 测试是否已成功为每个安全子域安装SSL证书。
1. 监测SSL证书的有效期。
1. 更新Adobe Campaign中的任何特定配置。

## 详细流程

### 先决条件

您必须识别域名和功能（跟踪、镜像页面、Web应用程序等） 来保护。
>[!NOTE]
>
>Adobe有助于定义要涉及的域名和函数。 有关更多信息，请与您的Adobe客户团队联系。

### 步骤1 — 获取CSR文件

要获取CSR（证书签名请求）文件，请执行以下步骤。

* 如果您有权访问[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hans)，请按照[此页面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=zh-Hans#subdomains-and-certificates)上的说明从控制面板生成并下载CSR文件。

* 否则，请通过https://adminconsole.adobe.com/创建支持工单，以从Adobe客户关怀部门获取CSR文件所需的子域。

以下是一些可遵循的最佳实践：

* 为每个委派的子域引发一个请求。
* 可以将多个子域合并到单个CSR请求中，但只能在同一环境中这样做。 例如，在Campaign Classic中，营销服务器、[中间源服务器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html)和[执行实例](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance)是三个不同的环境。
* 在续订任何SSL证书之前，您必须获取新的CSR。 请勿使用一年或更久以前的旧CSR文件。

您需要提供以下信息。

>[!CAUTION]
>
>必须填写下表中指示的所有字段。 否则，无法处理CSR请求。

**要向Adobe团队提供帮助的信息：**

| 要提供的信息 | 示例值 | 注释 |
|--- |--- |--- |
| 客户名称 | 我的公司 | 您的组织的名称。 Adobe使用此字段来跟踪您的请求（它不会成为CSR/SSL证书的一部分）。 |
| Adobe Campaign环境URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign实例URL |
| 公用名[CN] | t.subdomain.customer.com | 这可以是任何相关的域，但通常是跟踪域。 |
| 使用者替代名称[SAN] | t.subdomain.customer.com | 确保将跟踪子域作为SAN包括在内。 |
| 使用者替代名称[SAN] | m.subdomain.customer.com |
| 使用者替代名称[SAN] | res.subdomain.customer.com |

**要由IT/SSL内部团队提供的信息：**

| 要提供的信息 | 示例值 | 注释 |
|--- |--- |--- |
| 国家/地区[C] | US | 这必须是两个字母的代码。 在[此处](https://www.ssl.com/csrs/country_codes/)访问完整的国家/地区列表。</br>*注意：对于英国，请使用GB （而不是UK）。* |
| 省/市/自治区名称[ST] | 伊利诺伊 | 如果适用。 该值必须是全名，而不是缩写。 |
| 城市/地区名称[L] | 芝加哥 |
| 组织名称[O] | ACME |
| 组织单位名称[OU] | IT |

>[!NOTE]
>
>将“subdomain.customer.com”替换为您委派的子域，并将其他示例值替换为您相应的值。

### 步骤2 — 验证CSR文件

在提交您的请求及相关信息后，Adobe会生成证书签名请求(CSR)文件，并会向您提供该文件。

生成的CSR文件中的文本必须以&#x200B;**&quot;-----BEGIN CERTIFICATE REQUEST-----&quot;**&#x200B;开头。

从Adobe收到CSR文件后，请执行以下步骤：

1. 将CSR文件文本复制并粘贴到在线解码器中，如https://www.sslshopper.com/csr-decoder.html、<!--https://www.certlogik.com/decoder/,-->或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux计算机上本地使用*OpenSSL*&#x200B;命令。
1. 验证所有检查是否成功。
1. 检查是否包含正确的参数和域名。
1. 检查所有其他数据与您在提交请求时提供的详细信息是否匹配。

### 步骤3 — 生成SSL证书

提供CSR文件后，您必须使用CSR文件为相应的域购买并生成SSL证书。

* SSL证书：
   * 必须采用Apache PEM格式；
   * 不应超过2048位；
   * 必须由有效的CA（证书颁发机构）签署；
   * 必须包括CSR文件中提到的所有SAN （主题替代名称）。
* 如果有一个或多个中间证书，则必须提供根证书和所有要Adobe的中间证书。
* 您可以设置任意证书有效期，但Adobe建议选择足够长的时间（例如两年）。

>[!NOTE]
>
>如果您使用自己的内部工具或CA提供的门户来请求证书，请确保使用与CSR请求中提供的相同的详细信息，以避免证书生成过程中的任何延迟或差异。

### 步骤4 — 验证SSL证书

生成SSL证书后，您必须先验证该证书，然后再将其发送到Adobe。 要实现此目的，请执行以下步骤：

1. 确保证书具有.pem扩展名。 如果不是这种情况，请将其转换为PEM格式。 您可以使用&#x200B;*OpenSSL*&#x200B;进行转换。
1. 确认证书以&#x200B;**&quot;-----BEGIN CERTIFICATE-----&quot;**&#x200B;开头。
1. 将证书文本复制到在线解码器中，如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux计算机上本地使用*OpenSSL*&#x200B;命令。 有关详细信息，请参阅[此外部页面](https://www.shellhacks.com/decode-ssl-certificate/)。
1. 确保正确解析证书，包括公用名、SAN、颁发者和有效期。
1. 如果SSL证书验证成功，请使用[此网站](https://www.sslshopper.com/certificate-key-matcher.html)检查证书是否与CSR匹配：选择&#x200B;**检查CSR和证书是否匹配**，并在相应的字段中输入证书和您的CSR。 它们应该匹配。

### 步骤5 — 请求安装SSL证书

* 如果您有权访问[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hans)，请按照[此页面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html?lang=zh-Hans#installing-ssl-certificate)上的说明将证书上传到控制面板。

* 否则，请通过https://adminconsole.adobe.com/创建另一个支持票证，以请求Adobe在Adobe服务器上安装证书。

您需要提供：

* 证书文件、根证书和任何中间证书（附加到票证），最好采用Apache PEM格式。
* 为CSR引发的上一个支持票证的编号。
* 为CSR票证提供的相同数据（包括公用名称、实例URL、省/自治区/直辖市、组织名称、组织单位名称等）。

### 步骤6 — 测试SSL证书的安装

安装SSL证书并经Adobe客户关怀部门确认后，确保为所有URL成功安装了该证书。

在关闭SSL安装票证之前，请执行以下测试。 另请确保按照[此部分](#update-configuration)中的说明更新任何特定配置。

在浏览器中导航到以下URL（将“subdomain.customer.com”替换为您的子域）：

* https://subdomain.customer.com/r/test （仅适用于[Web应用程序](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html?lang=zh-Hans)子域 — 不适用于电子邮件子域）
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的结果会提供环境信息， URL中的地址栏表示连接是安全的。 例如，您可以在Google Chrome中看到以下消息：

![](../../help/assets/ssl-google-successful-result.png)

如果未正确安装SSL证书，则会显示以下警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步骤7 — 检查证书有效期

您可以在浏览器中检查证书的有效期。 例如，在Google Chrome中，单击&#x200B;**安全** > **证书**。

检查有效期是您的责任。 Adobe建议您实施用于监视证书到期的进程。 详细了解SSL证书在[本文](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/)后过期时会发生什么情况。

* 创建支持票证以在证书到期日期至少两周前请求更新的证书。 除非CSR详细信息已更改，否则您无需请求其他CSR。

* 如果您有权访问[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hans)，并且您的环境由AWS环境中的Adobe托管，则可以使用该控制面板在证书过期前续订证书。 有关详细信息，请参阅[此部分](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates)。

### 步骤8 — 更新任何特定配置 {#update-configuration}

在您确信请求的SSL证书已正确安装后，您可以将Adobe Campaign中的所有引用从HTTP更新为HTTPS。

>[!NOTE]
>
>对于Campaign Classic，要更新的URL主要位于[部署向导](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard)和[外部帐户](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/accessing-external-database/external-accounts.html)（跟踪、镜像页和公共资源域）中。 有关Campaign Standard，请参阅[品牌策略配置](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity)。

更新配置后，将使用HTTPS URL而不是HTTP发送新电子邮件。 要检查URL现在是否安全，您可以快速执行以下测试：

* 从Adobe Campaign上传图像。 上传图像后，返回的URL应为HTTPS。
* 创建测试电子邮件投放，包括镜像页面链接、某些图像、文本和退订链接。 将电子邮件发送到外部电子邮件ID（如您的Gmail地址）。 收到后，打开电子邮件并确保电子邮件中的所有链接都以其HTTPS表单（而不是HTTP）正确打开，且不会出现任何SSL证书警告或错误。

## 产品特定资源

**Campaign Classic**

* [控制面板：添加SSL证书（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html) — 了解如何添加SSL证书以保护子域。

**Campaign Standard**

* [控制面板：添加SSL证书（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html) — 了解如何添加SSL证书以保护子域。
