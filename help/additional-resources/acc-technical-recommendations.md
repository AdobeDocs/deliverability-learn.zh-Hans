---
title: Campaign Classic — 技术建议
description: 探索可用于提高Adobe Campaign Classic交付率的技术、配置和工具。
feature: 付诸实践
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '1579'
ht-degree: 1%

---


# Campaign Classic — 技术建议{#technical-recommendations}

下面列出了在使用Adobe Campaign Classic时可用于提高交付率的多种技术、配置和工具。

## 配置 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign检查是否为IP地址提供了反向DNS，并且这正确地指向IP。

网络配置中的一个重要点是确保为每个传出消息的IP地址定义了正确的反向DNS。 这意味着对于给定的IP地址，存在具有匹配DNS（A记录）的反向DNS记录（PTR记录），该记录循环回初始IP地址。

在与某些ISP打交道时，反向DNS的域选择会产生影响。 特别是，AOL只接受与反向DNS在同一域中具有地址的反馈循环（请参阅[反馈循环](#feedback-loop)）。

>[!NOTE]
>
>可以使用[此外部工具](https://mxtoolbox.com/SuperTool.aspx)验证域的配置。

### MX规则{#mx-rules}

MX规则（邮件eXchanger）是管理发送服务器与接收服务器之间的通信的规则。

更准确地说，它们用于控制Adobe CampaignMTA（邮件传输代理）向每个电子邮件域或ISP（例如hotmail.com、comcast.net）发送电子邮件的速度。 这些规则通常基于ISP发布的限制（例如，每个SMTP连接不包含20条以上的邮件）。

>[!NOTE]
>
>有关Adobe Campaign Classic中MX管理的详细信息，请参阅[本节](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration)。

### TLS {#tls}

TLS（传输层安全性）是一种加密协议，可用于保护两个电子邮件服务器之间的连接并保护电子邮件内容不被除预期收件人之外的任何人读取。

### 发件人的域{#sender-domain}

要定义用于HELO命令的域，请编辑实例的配置文件(conf/config-instance.xml)并定义“localDomain”属性，如下所示：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM域是用于技术弹回邮件的域。 此地址在部署向导中或通过NmsEmail_DefaultErrorAddr选项定义。

### SPF记录{#dns-configuration}

SPF记录当前可以在DNS服务器上定义为TXT类型记录（代码16）或SPF类型记录（代码99）。 SPF记录采用字符串形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

定义两个IP地址12.34.56.78和12.34.56.79，即有权发送域的电子邮件。 **~** all意味着任何其他地址应解释为SoftFail。

Recommendations，用于定义SPF记录：

* 最后添加&#x200B;**~all**(SoftFail)或&#x200B;**-all**(Fail)以拒绝除所定义服务器之外的所有服务器。 如果没有此功能，服务器将能够构建此域（使用中性评估）。
* 请勿添加&#x200B;**ptr**（openspf.org建议您不要这样做，因为这样做成本高昂且不可靠）。

>[!NOTE]
>
>在[本节](/help/additional-resources/authentication.md#spf)中了解有关SPF的更多信息。

## 身份验证

>[!NOTE]
>
>了解有关[本节](/help/additional-resources/authentication.md)中不同形式的电子邮件身份验证的更多信息。

### DKIM {#dkim-acc}

>[!NOTE]
>
>对于托管或混合安装，如果您已升级到[增强的MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)，则DKIM电子邮件身份验证签名由增强的MTA对所有域的所有消息完成。

将[DKIM](/help/additional-resources/authentication.md#dkim)与Adobe Campaign Classic一起使用需要以下先决条件：

**Adobe Campaign选项声明**:在Adobe Campaign中，DKIM私钥基于DKIM选择器和域。当前无法为具有不同选择器的同一域/子域创建多个私钥。 无法定义平台或电子邮件中身份验证必须使用哪个选择器域/子域。 该平台将选择其中一个私钥，这意味着身份验证很有可能失败。

* 如果已为Adobe Campaign实例配置DomainKeys，您只需在[域管理规则](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules)中选择&#x200B;**dkim**。 否则，请按照与DomainKeys（替换DKIM）相同的配置步骤（私有/公钥）。
* 无需为DKIM是DomainKeys的改进版本，因此对同一域同时启用DomainKeys和DKIM。
* 以下域当前验证DKIM:AOL，Gmail。

## 反馈循环{#feedback-loop-acc}

反馈循环的工作方式是在ISP级别为用于发送消息的一系列IP地址声明一个给定的电子邮件地址。 ISP将以类似方式向此邮箱发送邮件，这些邮件被收件人报告为垃圾邮件。 平台应配置为阻止向已投诉的用户进行未来投放。 即使他们未使用正确的退出链接，也必须不再与他们联系。 ISP将在其中添加IP地址，这是基于这些抱怨阻止列表的。 根据ISP的不同，约1%的投诉率将导致IP地址被阻塞。

目前正在制定一个标准来定义反馈循环消息的格式：[滥用反馈报告格式(ARF)](https://tools.ietf.org/html/rfc6650)。

为实例实现反馈循环需要：

* 专用于实例的邮箱，该邮箱可能是弹回邮箱
* 专用于实例的IP发送地址

在Adobe Campaign中实现简单的反馈循环使用弹回消息功能。 反馈循环邮箱用作跳回邮箱，并定义规则来检测这些邮件。 将邮件报告为垃圾邮件的收件人的电子邮件地址会添加到隔离列表。

* 在&#x200B;**[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]**&#x200B;中创建或修改弹出邮件规则&#x200B;**Feedback_loop**，原因为&#x200B;**Recombed**&#x200B;和类型&#x200B;**Hard**。
* 如果为反馈循环专门定义了邮箱，请通过在&#x200B;**[!UICONTROL Administration > Platform > External accounts]**&#x200B;中新建一个外部Bounce Meals帐户来定义参数以访问该邮箱。

该机制可立即处理投诉通知。 要确保此规则正常工作，您可以暂时取消激活帐户以使其不收集这些邮件，然后手动检查反馈循环邮箱的内容。 在服务器上，执行以下命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果强制您对多个实例使用一个反馈循环地址，则必须：

* 复制在任意数量的邮箱上接收的消息，
* 让每个邮箱由一个实例选取，
* 配置实例，以便它们只处理与它们相关的消息：实例信息包含在由Adobe Campaign发送的消息的Message-ID头中，因此也位于反馈循环消息中。 只需在实例配置文件中指定&#x200B;**checkInstanceName**&#x200B;参数即可（默认情况下，该实例不会验证，这可能导致某些地址被错误隔离）：

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign的可交付性服务管理您对以下ISP的反馈循环服务的订阅:AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Terra、UnitedOnline、USA、XSA4ALL，雅虎，Yandex，Zoho。

## 列表 — 取消订阅{#list-unsubscribe}

### 关于列表 — 取消订阅{#about-list-unsubscribe}

必须添加名为&#x200B;**列表-Unsubscribe**&#x200B;的SMTP头，以确保最佳的可交付性管理。

此标题可用作“报告为垃圾邮件”图标的替代内容。 它将在电子邮件界面中显示为退订链接。

使用此功能有助于保护您的声誉，反馈将作为退订执行。

要使用“列表取消订阅”，您必须输入类似于以下命令行：

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>上例基于收件人表。 如果从另一个表执行数据库实现，请确保使用正确的信息重写命令行。

以下命令行可用于创建动态&#x200B;**列表-Unsubscribe**:

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail、Outlook.com和Microsoft Outlook支持此方法，并且其界面中直接提供取消订阅按钮。 这种技术降低了投诉率。

您可以通过以下任一方式实现&#x200B;**列表-Unsubscribe**:

* 直接[在投放模板](#adding-a-command-line-in-a-delivery-template)中添加命令行
* [创建分类规则](#creating-a-typology-rule)

### 在投放模板{#adding-a-command-line-in-a-delivery-template}中添加命令行

必须在电子邮件SMTP头的其他部分添加命令行。

此添加操作可在每封电子邮件中或现有投放模板中完成。 您还可以创建包含此功能的新投放模板。

### 创建分类规则{#creating-a-typology-rule}

规则必须包含生成命令行的脚本，并且必须包含在电子邮件标头中。

>[!NOTE]
>
>我们建议创建类型规则:“列表取消订阅”功能将自动添加到每封电子邮件中。

1. 列表 — 取消订阅：&lt;mailto:unsubscribe@domain.com>

   单击&#x200B;**取消订阅**&#x200B;链接可打开用户的默认电子邮件客户端。 此类型规则必须添加到用于创建电子邮件的类型学中。

1. 列表 — 取消订阅：`<https://domain.com/unsubscribe.jsp>`

   单击&#x200B;**取消订阅**&#x200B;链接会将用户重定向到您的退订表单。

   示例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>了解如何在[此部分](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules)的Adobe Campaign Classic中创建类型规则。

## 电子邮件优化{#email-optimization}

### SMTP {#smtp}

SMTP（简单邮件传输协议）是用于电子邮件传输的因特网标准。

未由规则检查的SMTP错误列在&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]**&#x200B;文件夹中。 默认情况下，这些错误消息被解释为不可到达软错误。

如果要正确限定来自SMTP服务器的反馈，则必须识别最常见的错误，并在&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]**&#x200B;中添加相应的规则。 否则，平台将执行不必要的重试(未知用户情况)，或在给定数量的测试后错误地将某些收件人放在隔离中。

### 专用IP {#dedicated-ips}

Adobe为每位客户提供专用的IP战略，以便提高信誉和优化投放性能。
