---
title: Campaign Classic - 技术建议
description: 探索可用于提高Adobe Campaign Classic可投放率的技术、配置和工具。
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

下面列出了在使用Adobe Campaign Classic时可用于提高可投放性的几种技术、配置和工具。

## 配置 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign检查是否为IP地址提供了反向DNS，并且这正确指向IP。

网络配置中很重要的一点是确保为传出消息的每个IP地址定义了一个正确的反向DNS。 这意味着，对于给定的IP地址，存在反向DNS记录（PTR记录），其中匹配的DNS（A记录）循环返回到初始IP地址。

处理某些ISP时，反向DNS的域选择会产生影响。 特别是AOL，它只接受地址与反向DNS位于同一域中的反馈循环(请参阅 [反馈环](#feedback-loop))。

>[!NOTE]
>
>您可以使用 [此外部工具](https://mxtoolbox.com/SuperTool.aspx) 验证域的配置。

### MX规则 {#mx-rules}

MX规则（邮件交换器）是管理发送服务器和接收服务器之间通信的规则。

更准确地说，它们用于控制Adobe Campaign MTA（消息传输代理）向每个单独的电子邮件域或ISP(例如，hotmail.com、comcast.net)发送电子邮件的速度。 这些规则通常基于ISP发布的限制（例如，每个SMTP连接不包括超过20条消息）。

>[!NOTE]
>
>有关Adobe Campaign Classic中MX管理的更多信息，请参阅 [本节](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration).

### TLS {#tls}

TLS（传输层安全性）是一种加密协议，可用于保护两个电子邮件服务器之间的连接，并保护电子邮件的内容不被目标收件人以外的任何人读取。

### 发件人域 {#sender-domain}

要定义用于HELO命令的域，请编辑实例的配置文件(conf/config-instance.xml)并定义“localDomain”属性，如下所示：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM域是技术退回邮件中使用的域。 此地址是在部署向导中或通过NmsEmail_DefaultErrorAddr选项定义的。

### SPF记录 {#dns-configuration}

目前，SPF记录可在DNS服务器上定义为TXT类型记录（代码16）或SPF类型记录（代码99）。 SPF记录采用字符串形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

将两个IP地址12.34.56.78和12.34.56.79定义为有权为该域发送电子邮件。 **~all** 表示任何其他地址都应解释为SoftFail。

用于定义SPF记录的Recommendations：

* 添加 **~all** (SoftFail)或 **-all** （失败）以拒绝除已定义服务器之外的所有服务器。 否则，服务器将能够伪造此域（使用中性评估）。
* 不添加 **ptr** (openspf.org建议不要这么做，因为这样做成本高昂且不可靠)。

>[!NOTE]
>
>在中了解有关SPF的更多信息 [本节](/help/additional-resources/authentication.md#spf).

## 身份验证

>[!NOTE]
>
>在中了解关于不同形式的电子邮件身份验证的更多信息 [本节](/help/additional-resources/authentication.md).

### DKIM {#dkim-acc}

>[!NOTE]
>
>对于托管或混合安装，如果已升级到 [增强型MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)，DKIM电子邮件身份验证签名由Enhanced MTA为所有域的所有邮件完成。

使用 [DKIM](/help/additional-resources/authentication.md#dkim) 与Adobe Campaign Classic配合使用需要满足以下先决条件：

**Adobe Campaign选项声明**：在Adobe Campaign中，DKIM私钥基于DKIM选择器和域。 当前无法为使用不同选择器的同一域/子域创建多个私钥。 无法定义哪一个选择器域/子域必须用于平台或电子邮件中的身份验证。 平台可以选择其中一个私钥，这意味着身份验证很有可能失败。

* 如果您已为Adobe Campaign实例配置了域密钥，则只需选择 **dkim** 在 [域管理规则](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). 如果没有，请执行与DomainKeys（取代DKIM）相同的配置步骤（私钥/公钥）。
* 由于DKIM是DomainKeys的改进版本，因此不必为同一域同时启用DomainKeys和DKIM。
* 以下域当前验证DKIM：AOL、Gmail。

## 反馈环 {#feedback-loop-acc}

反馈循环的工作方式是在ISP级别声明用于发送消息的一系列IP地址的给定电子邮件地址。 ISP将按照与退回邮件类似的方式发送至此邮箱，退回邮件是指收件人报告为垃圾邮件的邮件。 平台应配置为阻止将来向投诉用户投放内容。 即使他们未使用正确的选择退出链接，也不要再与他们联系，这一点很重要。 阻止列表基于这些投诉，ISP会将IP地址添加到其。 根据ISP的不同，投诉率约为1%将导致阻止IP地址。

目前正在起草一个标准以定义反馈循环消息的格式： [滥用反馈报告格式(ARF)](https://tools.ietf.org/html/rfc6650).

为实例实施反馈循环需要：

* 专用于实例的邮箱，可能是退回邮箱
* 专用于实例的IP发送地址

在Adobe Campaign中实施简单的反馈循环时，会使用退回消息功能。 反馈循环邮箱用作退回邮箱，并定义规则以检测这些邮件。 将消息报告为垃圾邮件的收件人的电子邮件地址添加到隔离列表。

* 创建或修改退回邮件规则， **反馈循环**，在 **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** 原因如下 **已拒绝** 和类型 **硬**.
* 如果专门为反馈循环定义了邮箱，请通过在中创建新外部退回邮件帐户来定义用于访问邮箱的参数 **[!UICONTROL Administration > Platform > External accounts]**.

该机制可立即运作，以处理投诉通知。 要确保此规则正常工作，您可以暂时停用帐户以便它们不收集这些邮件，然后手动检查反馈循环邮箱的内容。 在服务器上，执行以下命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果强制为多个实例使用一个反馈循环地址，则您必须：

* 复制在任意数量的邮箱上接收的邮件，
* 让每个邮箱都由一个实例接收，
* 配置实例，使其仅处理与其相关的消息：实例信息包含在Adobe Campaign发送的消息的消息ID标头中，因此也位于反馈循环消息中。 只需指定 **checkInstanceName** 参数（默认情况下，不会验证实例，这可能会导致错误隔离某些地址）：

  ```
  <serverConf>
    <inMail checkInstanceName="true"/>
  </serverConf>
  ```

Adobe Campaign的可投放性服务管理您对以下ISP的反馈环路服务的订购：AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、Hotmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Telenor、Terra、UnitedOnline、USA、XS4ALL、Yahoo、Yahoo。

## 列表 — 取消订阅 {#list-unsubscribe}

### 关于列表取消订阅 {#about-list-unsubscribe}

添加名为的SMTP标头 **列表 — 取消订阅** 是确保最佳可投放性管理的必备条件。

此标头可用作“报告为垃圾邮件”图标的替代项。 它将在电子邮件界面中显示为退订链接。

使用此功能有助于保护您的声誉，并且反馈将作为退订执行。

要使用List-Unsubscribe，必须输入类似于下面的命令行：

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>以上示例基于收件人表。 如果数据库实施是从另一个表中完成的，请确保用正确的信息重写命令行。

以下命令行可用于创建动态 **列表 — 取消订阅**：

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail、Outlook.com和Microsoft Outlook支持此方法，并且其界面中直接提供了取消订阅按钮。 这种技术降低了投诉率。

您可以实施 **列表 — 取消订阅** 按以下任一方式执行：

* 直接 [在投放模板中添加命令行](#adding-a-command-line-in-a-delivery-template)
* [创建分类规则](#creating-a-typology-rule)

### 在投放模板中添加命令行 {#adding-a-command-line-in-a-delivery-template}

必须在电子邮件的SMTP标头的附加部分中添加命令行。

可以在每个电子邮件或现有投放模板中完成此添加。 您还可以创建包含此功能的新投放模板。

### 创建分类规则 {#creating-a-typology-rule}

规则必须包含生成命令行的脚本，并且必须包含在电子邮件标头中。

>[!NOTE]
>
>我们建议创建分类规则：将在每封电子邮件中自动添加List-Unsubscribe功能。

1. 列表 — 取消订阅： &lt;mailto:unsubscribe domain.com=&quot;&quot;>

   单击 **取消订阅** 链接会打开用户的默认电子邮件客户端。 必须在用于创建电子邮件的分类中添加此分类规则。

1. 列表 — 取消订阅： `<https://domain.com/unsubscribe.jsp>`

   单击 **取消订阅** 链接会将用户重定向到您的退订表单。

   示例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>了解如何在Adobe Campaign Classic中创建分类规则 [本节](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

## 电子邮件优化 {#email-optimization}

### SMTP {#smtp}

SMTP（简单邮件传输协议）是用于电子邮件传输的Internet标准。

规则未检查的SMTP错误列在 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** 文件夹。 默认情况下，这些错误消息被解释为无法访问的可软错误。

必须确定最常见的错误，并在中添加相应的规则 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** 如果您希望正确地确认来自SMTP服务器的反馈。 否则，平台将执行不必要的重试（对于未知用户），或在给定数量的测试后错误地隔离某些收件人。

### 专用IP {#dedicated-ips}

Adobe为每位客户提供专门的IP策略，并增加IP以建立信誉和优化投放性能。
