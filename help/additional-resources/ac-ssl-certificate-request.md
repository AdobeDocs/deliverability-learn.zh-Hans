---
title: SSL证书请求过程
description: 了解如何在您委派给Adobe的子域上安装SSL证书。
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
exl-id: 8a78abd3-afba-49a7-a2ae-8b2c75326749
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '2266'
ht-degree: 1%

---

# SSL 证书请求进程

将域委派给Adobe以发送电子邮件后（请参阅[域名设置](/help/additional-resources/ac-domain-name-setup.md)），Adobe将创建特定子域并将其用于特定功能。

例如，如果您已将&#x200B;*email.example.com*&#x200B;委派给Adobe以发送电子邮件，则Adobe将创建如下子域：
* *t.email.example.com*  — 用于跟踪链接
* *m.email.example.com*  — 用于镜像页面
* *res.email.example.com*  — 用于托管资源（如图像）

建议&#x200B;**通过SSL(HTTPS)**&#x200B;保护这些域。 事实上，不安全的链接(HTTP)容易被拦截，并会在现代浏览器上标记警告。

要在这些子域上安装SSL证书，该过程包括请求CSR文件，然后购买SSL证书以供Adobe安装或续订。

>[!CAUTION]
>
>在安装SSL证书之前，请确保您了解[此页面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate)中列出的先决条件。
>
>Adobe最多仅支持2048位证书。 尚不支持4096位证书。

## 术语表

| 搜索词 | 描述 |
|--- |--- |
| CA（证书颁发机构） | SSL证书提供程序，用于在验证组织或个人的身份（如DigiCert、Symantec等）后向其颁发数字证书。<ul><li>受信任的CA通常被视为颁发根证书的第三方CA。</li><li>如果证书由使用证书的同一组织/公司签名，则即使证书是SSL证书（如自签名证书），也会被分类为不受信任的CA。</li></ul> |
| 链式证书 | 包括根证书和一个或多个中间证书的证书称为链（或链）证书。 |
| CSR（证书签名请求） | 申请SSL证书时给予证书颁发机构的编码文本块。 它通常在安装证书的服务器上生成。 |
| DER（可分辨编码规则） | 证书扩展类型。 .der扩展用于二进制DER编码证书。 这些文件还可能支持.cer或.crt扩展名。 |
| EV（扩展验证）证书 | EV证书是一种新型的证书，旨在防止网络钓鱼攻击。 它需要对您的业务和订购证书的人员进行延期验证。 |
| 高保证证书 | CA在验证域名所有权和有效业务注册后颁发高保证证书。 |
| 中间CA | 链式证书中包含的中间证书的证书颁发机构。 |
| 中间证书 | 证书颁发机构以树结构的形式颁发证书。 根证书是树中排名最前的证书。 您的证书与根证书之间的任何证书都称为链式证书或中间证书。 |
| 低保证证书 | 低保证证书（也称为经域验证的证书）仅包含证书中的域名（而不包括业务/组织名称）。 |
| PEM（隐私增强邮件） | 扩展名为.pem的证书，其中包含ASCII(Base64)数据。 此类证书以“ — — - BEGIN CERTIFICATE — — ”行开头。 |
| 根证书 | 证书颁发机构以树结构的形式颁发证书。 根证书是树中排名最前的证书。 |
| SAN（主题替代名称） | 主题的替代名称是其他主机名（站点、IP地址、通用名称等） 应作为单个SSL证书的一部分进行签名。 |
| 自签名证书 | 由创建证书的人员（而不是受信任的证书颁发机构）签名的证书。 自签名证书可以实现与由CA签名的证书相同级别的加密，但存在两个主要缺陷：<ul><li>访客的连接可能被劫持，使得攻击者能够查看所发送的所有数据（从而破坏了加密连接的目的）</li><li> 无法像可信证书一样吊销证书。</li></ul> |
| SSL（安全套接字层） | 在Web服务器和浏览器之间建立加密链接的标准安全技术。 |
| 通配符证书 | 通配符证书可以保护单个域名（如*.adobe.com）上无限数量的第一级子域。 |

## 主要步骤

1. 请求获取证书签名请求(CSR)文件，并提供所需的信息（国家/地区、州、城市、组织名称、组织单位名称等） Adobe。
1. 验证由Adobe生成的CSR文件，并验证您提供的所有信息均正确无误。
1. 使用CSR详细信息生成由受信任的认证中心<!--taking care of asking for using the subjectAltName SSL extension (SAN) if it is for several domain names, and get/purchase the resulting certificate (ideally) in PEM format for Apache server-->签名的证书。
1. 验证SSL证书并验证其与CSR匹配。
1. 将SSL证书提供给Adobe，由谁安装。
1. 测试是否已成功为每个安全子域安装SSL证书。
1. 监控SSL证书的有效期。
1. 更新Adobe Campaign中的任何特定配置。

## 详细流程

### 先决条件

您必须标识域名和函数（跟踪、镜像页面、Web应用程序等） 以保护安全。
>[!NOTE]
>
>Adobe有助于定义要涉及的域名和函数。 有关更多信息，请联系您的Adobe客户成功经理。

### 步骤1 — 获取CSR文件

要获取CSR（证书签名请求）文件，请执行以下步骤。

* 如果您有权访问[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hans)，请按照[此页面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#subdomains-and-certificates)上的说明，从控制面板中生成并下载CSR文件。

* 否则，请通过https://adminconsole.adobe.com/创建支持票证，以从Adobe客户关怀团队获取所需子域的CSR文件。

以下是一些要遵循的最佳实践：

* 每个委派的子域提出一个请求。
* 可以将多个子域合并到单个CSR请求中，但只能在同一环境中。 例如，在Campaign Classic中，营销服务器、[中间源服务器](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/install-campaign-on-prem/mid-sourcing-server.html)和[执行实例](https://experienceleague.adobe.com/docs/campaign-classic/using/transactional-messaging/configure-transactional-messaging/configuring-instances.html#execution-instance)是三个单独的环境。
* 在任何SSL证书续订之前，您必须获取新CSR。 请勿使用一年前或更久之前的旧CSR文件。

您需要提供以下信息。

>[!CAUTION]
>
>必须填写下表中指示的所有字段。 否则，无法处理CSR请求。

**向Adobe团队提供帮助的信息：**

| 要提供的信息 | 示例值 | 注释 |
|--- |--- |--- |
| 客户端名称 | My Company Inc. | 您的组织的名称。 此字段由Adobe用于跟踪您的请求（它不属于CSR/SSL证书的一部分）。 |
| Adobe Campaign环境URL | https://client-mid-prod1.campaign.adobe.com | Adobe Campaign实例URL。 |
| 通用名称[CN] | t.subdomain.customer.com | 这可以是任何相关域，但通常是跟踪域。 |
| 主题替代名称[SAN] | t.subdomain.customer.com | 确保将跟踪子域作为SAN包含在内。 |
| 主题替代名称[SAN] | m.subdomain.customer.com |
| 主题替代名称[SAN] | res.subdomain.customer.com |

**IT/SSL内部团队提供的信息：**

| 要提供的信息 | 示例值 | 注释 |
|--- |--- |--- |
| 国家/地区[C] | 美国 | 这必须是双字母代码。 在此处](https://www.ssl.com/csrs/country_codes/)访问完整的国家/地区列表。[</br>*注意：对于英国，请使用GB（而非UK）。* |
| 州（或省名）[ST] | 伊利诺伊州 | 如果适用。 值必须是全名，而不是缩写。 |
| 城市/地区名称[L] | 芝加哥 |
| 组织名称[O] | ACME |
| 组织单位名称[OU] | IT |

>[!NOTE]
>
>将“subdomain.customer.com”替换为您的委派子域，而其他示例值则替换为相应的值。

### 第2步 — 验证CSR文件

在提交包含相关信息的请求后，Adobe会生成证书签名请求(CSR)文件，并为您提供该文件。

生成的CSR文件中的文本必须以&#x200B;**&quot;—BEGIN CERTIFICATE REQUEST—&quot;**&#x200B;开头。

从Adobe收到CSR文件后，请执行以下步骤：

1. 将CSR文件文本复制并粘贴到联机解码器中，如https://www.sslshopper.com/csr-decoder.html、<!--https://www.certlogik.com/decoder/,-->或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux计算机上本地使用*OpenSSL*&#x200B;命令。 有关更多信息，请参阅[此外部页面](https://www.question-defense.com/2009/09/22/use-openssl-to-verify-the-contents-of-a-csr-before-submitting-for-a-ssl-certificate)。
1. 确认所有检查均成功。
1. 检查是否包含正确的参数和域名。
1. 检查所有其他数据是否与您在提交请求时提供的详细信息相匹配。

### 步骤3 — 生成SSL证书

提供CSR文件后，您必须使用CSR文件为相应的域购买并生成SSL证书。

* SSL证书：
   * 必须为Apache PEM格式；
   * 长度不应超过2048位；
   * 必须由有效的CA（认证机构）签署；
   * 必须包括CSR文件中所述的所有SAN（主体替代名称）。
* 如果有一个或多个中间证书，则必须提供根证书和所有中间证书才能Adobe。
* 您可以设置任何证书的有效期，但Adobe建议选择足够长的期限（例如，两年）。

>[!NOTE]
>
>如果您使用自己的内部工具或CA提供的门户来请求证书，请确保使用CSR请求中提供的相同详细信息，以避免证书生成过程中的任何延迟或差异。

### 步骤4 — 验证SSL证书

生成SSL证书后，您必须先验证该证书，然后再将其发送到Adobe。 要实现此目的，请执行以下步骤：

1. 确保证书的扩展名为.pem。 如果不是，请将其转换为PEM格式。 您可以使用&#x200B;*OpenSSL*&#x200B;进行转换。
1. 确认证书以&#x200B;**&quot;—BEGIN CERTIFICATE—&quot;**&#x200B;开头。
1. 将证书文本复制到联机解码器中，如https://www.sslshopper.com/certificate-decoder.html或https://www.entrust.net/ssl-technical/csr-viewer.cfm。
或者，您也可以在Linux计算机上本地使用*OpenSSL*&#x200B;命令。 有关更多信息，请参阅[此外部页面](https://www.shellhacks.com/decode-ssl-certificate/)。
1. 确保证书正确解析，包括通用名称、SAN、颁发者和有效期。
1. 如果SSL证书验证成功，请使用[此网站](https://www.sslshopper.com/certificate-key-matcher.html)检查证书是否与CSR匹配：选择&#x200B;**检查CSR和证书是否与**&#x200B;匹配，然后在相应字段中输入证书和CSR。 它们应该匹配。

### 步骤5 — 请求安装SSL证书

* 如果您有权访问[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，请按照[此页面](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/renewing-subdomain-certificate.html#installing-ssl-certificate)上的说明将证书上传到控制面板。

* 否则，请通过https://adminconsole.adobe.com/创建另一个支持票证，以请求Adobe在Adobe服务器上安装证书。

您需要提供：

* 证书文件、根证书和任何中间证书（附加到票证），最好采用Apache PEM格式。
* 为CSR引发的上一个支持票证的编号。
* 为CSR票证提供的数据（包括通用名称、实例URL、州、城市/地区、组织名称、组织单位名称等）。

### 步骤6 — 测试SSL证书安装

安装SSL证书并由Adobe客户关怀确认后，请确保已为所有URL成功安装该证书。

在关闭SSL安装票证之前，请执行以下测试。 另请确保按照[此部分](#update-configuration)中的说明更新任何特定配置。

导航到浏览器中的以下URL（将“subdomain.customer.com”替换为您的子域）：

* https://subdomain.customer.com/r/test（仅适用于[web应用程序](https://experienceleague.adobe.com/docs/campaign-classic/using/designing-content/web-applications/about-web-applications.html)子域 — 不适用于电子邮件子域）
* https://t.subdomain.customer.com/r/test
* https://m.subdomain.customer.com/r/test
* https://res.subdomain.customer.com/r/test

成功的结果会提供环境信息，而URL中的地址栏表示连接安全。 例如，您可以在Google Chrome中看到以下消息：

![](../../help/assets/ssl-google-successful-result.png)

如果SSL证书安装不正确，则会显示以下警告：

![](../../help/assets/ssl-google-unsuccessful-result.png)

### 步骤7 — 检查证书的有效期

您可以在浏览器中检查证书的有效期。 例如，在Google Chrome中，单击&#x200B;**Secure** > **Certificate**。

您有责任检查有效期。 Adobe建议您实施一个流程来监控证书到期。 了解有关SSL证书在[过期后所发生情况的更多信息，请参阅本文](https://www.thesslstore.com/blog/what-happens-when-your-ssl-certificate-expires/)。

* 创建支持票证，以在证书过期日期至少提前两周请求更新的证书。 除非CSR详细信息发生更改，否则您无需请求其他CSR。

* 如果您有权访问[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html)，并且您的环境由AWS环境中的Adobe托管，则可以使用控制面板在证书过期之前续订证书。 在[此部分](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-ssl-certificates.html#monitoring-certificates)中了解详情。

### 步骤8 — 更新任何特定配置 {#update-configuration}

确信所请求的SSL证书安装正确后，您便可以将Adobe Campaign中的所有引用从HTTP更新为HTTPS。

>[!NOTE]
>
>对于Campaign Classic，要更新的URL主要位于[部署向导](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/initial-configuration/deploying-an-instance.html#deployment-wizard)和[外部帐户](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/external-accounts.html#installing-campaign-classic)（跟踪、镜像页面和公共资源域）中。 有关Campaign Standard，请参阅[品牌配置](https://experienceleague.adobe.com/docs/campaign-standard/using/administrating/application-settings/branding.html#about-brand-identity)。

更新配置后，将使用HTTPS URL而不是HTTP发送新电子邮件。 要检查URL现在是否安全，您可以快速执行以下测试：

* 从Adobe Campaign上传图像。 上传图像后，返回的URL应为HTTPS。
* 创建测试电子邮件投放，其中包括镜像页面链接、某些图像、文本和退订链接。 将电子邮件发出到外部电子邮件ID（如您的Gmail地址）。 收到后，请打开电子邮件，并确保电子邮件内的所有链接以HTTPS格式（而非HTTP）正确打开，且没有任何SSL证书警告或错误。

## 产品特定资源

**Campaign Classic**

* [控制面板:添加SSL证书（教程）](https://experienceleague.adobe.com/docs/campaign-classic-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 了解如何添加SSL证书以保护子域。

**Campaign Standard**

* [控制面板:添加SSL证书（教程）](https://experienceleague.adobe.com/docs/campaign-standard-learn/control-panel/subdomains-and-certificates/adding-ssl-certificates.html)  — 了解如何添加SSL证书以保护子域。
