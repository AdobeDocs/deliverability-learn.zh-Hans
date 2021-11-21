---
title: Campaign Classic - 技术建议
description: 了解使用Adobe Campaign Classic提高可投放率的技术、配置和工具。
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

下面列出了在使用Adobe Campaign Classic时可用于提高投放能力率的多种技术、配置和工具。

## 配置 {#configuration}

### 反向DNS {#reverse-dns}

Adobe Campaign会检查是否为IP地址提供了反向DNS，并且这是否正确指向了IP。

网络配置中的一个重要点是确保为传出消息的每个IP地址定义正确的反向DNS。 这意味着对于给定的IP地址，存在一个具有匹配DNS（A记录）的反向DNS记录（PTR记录），该记录循环回初始IP地址。

在处理某些ISP时，反向DNS的域选择会产生影响。 特别是，AOL只接受与反向DNS在同一域中具有地址的反馈循环(请参阅 [反馈循环](#feedback-loop))。

>[!NOTE]
>
>您可以使用 [此外部工具](https://mxtoolbox.com/SuperTool.aspx) 来验证域的配置。

### MX规则 {#mx-rules}

MX规则（邮件eXchanger）是管理发送服务器与接收服务器之间通信的规则。

更准确地说，它们用于控制Adobe Campaign MTA（消息传输代理）向每个单独的电子邮件域或ISP（例如hotmail.com、comcast.net）发送电子邮件的速度。 这些规则通常基于ISP发布的限制（例如，每个SMTP连接不包含超过20条消息）。

>[!NOTE]
>
>有关Adobe Campaign Classic中MX管理的更多信息，请参阅 [此部分](https://experienceleague.adobe.com/docs/campaign-classic/using/installing-campaign-classic/additional-configurations/email-deliverability.html#mx-configuration).

### TLS {#tls}

TLS（传输层安全性）是一种加密协议，可用于保护两个电子邮件服务器之间的连接，并保护电子邮件内容不受目标收件人以外的任何人读取。

### 发件人的域 {#sender-domain}

要定义用于HELO命令的域，请编辑实例的配置文件(conf/config-instance.xml)并定义“localDomain”属性，如下所示：

```
<serverConf>
  <shared>
    <dnsConfig localDomain="mydomain.net"/>
  </shared>
</serverConf>
```

MAIL FROM域是技术退回邮件中使用的域。 此地址在部署向导中或通过NmsEmail_DefaultErrorAddr选项定义。

### SPF记录 {#dns-configuration}

SPF记录当前可在DNS服务器上定义为TXT类型记录（代码16）或SPF类型记录（代码99）。 SPF记录采用字符串形式。 例如：

```
v=spf1 ip4:12.34.56.78/32 ip4:12.34.56.79/32 ~all
```

定义有权为域发送电子邮件的两个IP地址，即12.34.56.78和12.34.56.79。 **~all** 表示任何其他地址都应解释为SoftFail。

Recommendations，用于定义SPF记录：

* 添加 **~all** (SoftFail)或 **-all** （失败）最终拒绝除所定义服务器之外的所有服务器。 如果没有这一点，服务器将能够伪造此域（通过中性评估）。
* 不添加 **ptr** （openspf.org建议不要这样做，因为这样做成本高昂且不可靠）。

>[!NOTE]
>
>了解有关SPF的更多信息，请参阅 [此部分](/help/additional-resources/authentication.md#spf).

## 身份验证

>[!NOTE]
>
>进一步了解 [此部分](/help/additional-resources/authentication.md).

### DKIM {#dkim-acc}

>[!NOTE]
>
>对于托管安装或混合安装(如果已升级到 [增强的MTA](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/sending-emails/sending-an-email/sending-with-enhanced-mta.html#sending-messages)，所有域的所有消息的DKIM电子邮件身份验证签名由Enhanced MTA完成。

使用 [DKIM](/help/additional-resources/authentication.md#dkim) 使用Adobe Campaign Classic需要满足以下先决条件：

**Adobe Campaign期权声明**:在Adobe Campaign中，DKIM私钥基于DKIM选择器和域。 当前无法为具有不同选择器的同一域/子域创建多个私钥。 无法定义平台或电子邮件中身份验证都必须使用哪个选择器域/子域。 平台或者将选择其中一个私钥，这意味着身份验证很有可能失败。

* 如果您为Adobe Campaign实例配置了DomainKeys，则只需选择 **dkim** 在 [域管理规则](https://experienceleague.adobe.com/docs/campaign-classic/using/sending-messages/monitoring-deliveries/understanding-delivery-failures.html#email-management-rules). 如果没有，请执行与DomainKeys（取代DKIM）相同的配置步骤（私钥/公钥）。
* 对于DKIM是DomainKeys的改进版本，因此不必为同一域同时启用DomainKeys和DKIM。
* 以下域当前正在验证DKIM:AOL，Gmail。

## 反馈循环 {#feedback-loop-acc}

反馈循环在ISP级别为用于发送消息的IP地址范围声明给定的电子邮件地址。 ISP将以与退回邮件类似的方式向此邮箱发送收件人报告为垃圾邮件的邮件。 应该将平台配置为阻止将来向已投诉的用户交付内容。 即使他们未使用正确的选择退出链接，也必须不再与他们联系。 根据这些投诉，ISP将在其中添加一个IP地阻止列表址。 如果投诉率在1%左右，则会导致阻止IP地址，具体取决于ISP。

目前正在制定一个标准来定义反馈循环消息的格式：the [滥用反馈报告格式(ARF)](https://tools.ietf.org/html/rfc6650).

为实例实施反馈循环需要：

* 专用于实例的邮箱，可能是退回邮箱
* 专用于实例的IP发送地址

在Adobe Campaign中实施简单的反馈循环时，会使用退件消息功能。 反馈循环邮箱用作退回邮箱，并定义规则来检测这些邮件。 将邮件报告为垃圾邮件的收件人的电子邮件地址添加到隔离列表。

* 创建或修改退回邮件规则， **Feedback_loop**，在 **[!UICONTROL Administration > Campaign Management > Non deliverables Management > Mail rule sets]** 原因 **已拒绝** 和类型 **硬**.
* 如果为反馈循环专门定义了邮箱，请定义参数以通过在 **[!UICONTROL Administration > Platform > External accounts]**.

该机制立即开始处理投诉通知。 为确保此规则正常工作，您可以暂时停用帐户，以便它们不收集这些邮件，然后手动检查反馈循环邮箱的内容。 在服务器上，执行以下命令：

```
nlserver stop inMail@instance,
nlserver inMail -instance:instance -verbose.
```

如果您被强制对多个实例使用一个反馈循环地址，则必须：

* 复制在任意数量的邮箱上收到的消息，
* 让每个邮箱由一个实例提取，
* 配置实例，以便它们只处理与它们相关的消息：实例信息包含在Adobe Campaign发送的消息的消息ID标头中，因此也位于反馈循环消息中。 只需指定 **checkInstanceName** 实例配置文件中的参数（默认情况下，不会验证实例，这可能会导致某些地址被错误隔离）：

   ```
   <serverConf>
     <inMail checkInstanceName="true"/>
   </serverConf>
   ```

Adobe Campaign的可投放性服务可管理您对以下ISP的反馈循环服务的订购：AOL、BlueTie、Comcast、Cox、EarthLink、FastMail、Gmail、HostedEmail、Libero、Mail.ru、MailTrust、OpenSRS、QQ、RoadRunner、Synacor、Telenor、Terra、UniteOnline、USA、XS4ALL、Yaho、Yandex、Zoho。

## 列表取消订阅 {#list-unsubscribe}

### 关于List-Unsubscribe {#about-list-unsubscribe}

添加名为 **列表取消订阅** 是确保最佳投放能力管理的必备条件。

此标头可用作“报告为垃圾邮件”图标的替代内容。 它将在电子邮件界面中显示为退订链接。

使用此功能有助于保护您的声誉，并且将作为退订执行反馈。

要使用“列表取消订阅”，必须输入类似于以下的命令行：

```
List-Unsubscribe: mailto: client@newsletter.example.com?subject=unsubscribe?body=unsubscribe
```

>[!CAUTION]
>
>以上示例基于收件人表。 如果从另一个表完成了数据库实现，请确保用正确的信息重述命令行。

以下命令行可用于创建动态 **列表取消订阅**:

```
List-Unsubscribe: mailto: %=errorAddress%?subject=unsubscribe%=message.mimeMessageId%
```

Gmail、Outlook.com和Microsoft Outlook支持此方法，并且其界面中可直接使用取消订阅按钮。 这种技术可以降低投诉率。

您可以实施 **列表取消订阅** 按以下任一方式：

* 直接 [在投放模板中添加命令行](#adding-a-command-line-in-a-delivery-template)
* [创建分类规则](#creating-a-typology-rule)

### 在投放模板中添加命令行 {#adding-a-command-line-in-a-delivery-template}

必须在电子邮件SMTP标头的其他部分中添加命令行。

此添加操作可以在每封电子邮件中或现有投放模板中完成。 您还可以创建包含此功能的新投放模板。

### 创建分类规则 {#creating-a-typology-rule}

规则必须包含用于生成命令行的脚本，且必须包含在电子邮件标头中。

>[!NOTE]
>
>我们建议创建分类规则：列表取消订阅功能将自动添加到每封电子邮件中。

1. 列表取消订阅： &lt;mailto:unsubscribe domain.com=&quot;&quot;>

   单击 **取消订阅** 链接会打开用户的默认电子邮件客户端。 必须在用于创建电子邮件的分类中添加此分类规则。

1. 列表取消订阅： `<https://domain.com/unsubscribe.jsp>`

   单击 **取消订阅** 链接会将用户重定向到您的退订表单。

   示例：

   ![](../assets/s_tn_del_unsubscribe_param.png)

>[!NOTE]
>
>了解如何在Adobe Campaign Classic中创建分类规则(位于 [此部分](https://experienceleague.adobe.com/docs/campaign-classic/using/orchestrating-campaigns/campaign-optimization/about-campaign-typologies.html#typology-rules).

## 电子邮件优化 {#email-optimization}

### SMTP {#smtp}

SMTP（简单邮件传输协议）是电子邮件传输的Internet标准。

规则未检查的SMTP错误列在 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Delivery log qualification]** 文件夹。 默认情况下，这些错误消息被解释为不可到达的软错误。

必须识别最常见的错误，并在 **[!UICONTROL Administration]** > **[!UICONTROL Campaign Management]** > **[!UICONTROL Non deliverables Management]** > **[!UICONTROL Mail rule sets]** 如果您希望正确确定来自SMTP服务器的反馈。 如果没有这种情况，平台将执行不必要的重试（用户未知的情况），或在给定数量的测试后错误地将某些收件人置于隔离中。

### 专用IP {#dedicated-ips}

Adobe为每位客户提供专用的IP策略，并提升IP，以建立信誉并优化交付性能。
