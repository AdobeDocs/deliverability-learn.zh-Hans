---
title: 域名设置
description: 了解如何将子域委派到Adobe Campaign。
feature: Putting it in practice
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 1e539b5df54250a5927701009e7a9c84e5d73fae
workflow-type: tm+mt
source-wordcount: '2032'
ht-degree: 1%

---


# 域名设置

本文档介绍了域名设置和委派的业务和技术要求。 您将需要选择发送子域的电子邮件以及（可选）面向外部的子域来托管您所使用的Adobe平台的Web组件(登陆页、退出页面)。

>[!NOTE]
>
>您还可以使用控制面板（作为测试版提供）设置新子域。 请阅读[本节](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read)了解更多信息。

## 子域

借助Adobe，数字营销可真正成为情境引擎，推动您品牌的客户参与营销项目。  电子邮件仍是数字营销项目的基础。 然而，到达收件箱变得比以往更加困难。

创建电子邮件活动的子域允许品牌将不同类型的流量（例如营销与公司）隔离到特定IP池中，并具有特定域，这将加快[ IP预热过程](../../help/additional-resources/increase-reputation-with-ip-warming.md)并提高整体交付能力。 如果您共享一个域，并且该域被阻止或添加到阻止列表中，可能会影响您的公司邮件投放。 但是，特定于电子邮件营销通讯的域上的声誉问题或块将仅影响该电子邮件流。  将主域用作多个邮件流的发件人地址或“发件人”地址也可能会中断电子邮件身份验证，导致您的邮件被阻止或放入垃圾邮件文件夹中。

### 委托

域名委派是一种允许域名所有者的方法(技术上：DNS区域)，以委派其细分(技术上：DNS区域，可称为子区域)。 基本上，如果客户正在处理区域“example.com”，他可以将子区域“marketing.example.com”委派给Adobe Campaign。

这意味着Adobe Campaign的DNS服务器将仅对该区域拥有完全权限，而不是顶级域。 Adobe Campaign的DNS服务器将为该区域中域名的查询提供权威性答案，例如“t.marketing.example.com”本身，但不是“www.example.com”。

通过委托子域与Adobe Campaign一起使用，客户可以依赖Adobe来维护满足其电子邮件营销发送域行业标准可交付性要求所需的DNS基础结构，同时继续维护和控制其内部电子邮件域的DNS。  子域委派允许：

客户通过使用域名的DNS别名来保留其品牌图像
Adobe自主实施所有技术最佳做法，以在发送电子邮件时充分优化交付能力

## DNS设置选项

为了提供基于云的托管服务，Adobe强烈鼓励客户在部署Adobe Campaign时使用子域委派。  但是，Adobe确实为优惠客户端提供了用于配置DNS的替代选项 — CNAME设置。

| 选项 | 描述 | Adobe责任 | 客户责任 |
|--- |------- |--- |--- |
| 子域委托到Adobe Campaign | 客户端将子域(email.example.com)委派给Adobe。 在此方案中，Adobe能够通过控制和维护DNS的所有方面来将活动作为托管服务提供，而这些方面是发送、渲染和跟踪电子邮件活动所必需的。 | 对子域和Adobe Campaign所需的所有DNS记录进行完整管理。 | 正确将子域委派到Adobe |
| 使用 CNAME | 客户端创建子域并使用CNAME指向特定于Adobe的记录。  使用此设置，Adobe 和客户共同负责维护 DNS。 | 管理Adobe Campaign所需的DNS记录。 | 创建和控制子域以及创建/管理Adobe Campaign所需的CNAME记录。 |

## 所需的DNS记录

| 记录类型 | 用途 | 示例记录/内容 |
|--- |--- |--- |
| MX | 为传入邮件指定邮件服务器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF(TXT) | 发件人策略框架 | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.活动.adobe.com&quot; |
| DKIM(TXT) | DomainKeys已识别邮件 | <i>客户端。_domainkey.email.example.com</i></br>&quot;v=DKIM1;k=rsa;&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| DMARC(TXT) | 基于域的消息身份验证 | 报告与符合性 | _dmarc.email.example.com</br>&quot;v=DMARC1;p=none;rua=mailto:mailauth-reports@myemail.com&quot; |
| 主机记录(A) | 镜像页面、图像托管和跟踪链接，所有发送域 | m.email.example.com（位于123.111.100.99</br>t.email.example.com）（位于A 123.111.100.98</br>email.example.com中）00.97 |
| 反向DNS(PTR) | 将客户端IP地址映射到客户端品牌主机名 | 18.101.100.192.in-addr.arpa域名指针r18.email.example.com |
| CNAME | 为其他域名提供别名 | t1.email.example.com是 | t1.email.example.campaign.adobe.com |

## 设置要求

### 子域委派

这要求客户端在其DNS服务器中创建子域，并将此子域的名称服务器定义为由Adobe维护的服务器。  例如，主域名为“example.com”且要将“marketing.example.com”的管理委派给其电子邮件投放的Adobe的客户端必须实现此委派，才能将以下类型记录添加到其DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

域名的委派意味着此域将专门用于通过Adobe Campaign平台传送电子邮件，因此不能用于其他方式（例如，从其他电子邮件基础结构发送电子邮件）。

在设置过程中，Adobe将确保域连接到Adobe传入电子邮件基础架构，以便管理和处理返回到这些域（MX类型DNS记录配置）的反弹电子邮件。

### 使用 CNAME

如果客户端选择使用CNAME而不是将子域委派到Adobe，则在设置阶段，Adobe将提供要放置在客户端DNS服务器中的记录，并将在Adobe CampaignDNS服务器中配置相应的值。

## 部署的一般要求

实施新的企业营销解决方案时，需要面向外部的组件。  这些功能包括托管登陆页和Web表单、设置要跟踪的链接和网页、显示镜像页面以及配置退出页面。

虽然这些要求是通过由Adobe和客户托管的组件进行管理的，但它们包含URL，这些URL可以通过电子邮件的收件人查看。  为避免有指示基础技术解决方案或托管提供商的URL，可以设置子域以使此对电子邮件收件人透明。  例如，当查看URL(如http://www.customer.com/)时，域将为“www.customer.com”。  此子域为“www”。

### 子域要求

确定用于Adobe Campaign应用程序中的品牌URL(镜像页面和跟踪URL)的子域。  另外，决定电子邮件投放上每个子域的“发件人地址”、“发件人名称”和“回复地址”是什么。

填写下表，第一行仅是示例。

| Subdomain | 发件人地址 | 发件人名称 | 回复地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | 客户 | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* “回复地址”字段的目的是让收件人回复与“发件人地址”不同的地址。  虽然不是必填字段，但Adobe强烈建议“回复地址”有效并链接到受监视的邮箱。  此邮箱必须由客户托管。  它可以是一个支持邮箱，例如customercare@customer.com，在此处读取和响应电子邮件。
>* 如果客户未选择“回复地址”，则默认地址始终为`<tenant>-<type>-<env>@<subdomain>`。
>* 以这种方式设置“回复地址”时，回复将发送到未监控的邮箱。
>* 从Adobe Campaign发送电子邮件时，不会监控“发件人地址”邮箱，营销用户无法访问此邮箱。 Adobe Campaign也不会优惠在此邮箱中收到的自动回复或自动转发电子邮件的功能。
>* 活动发件人地址和错误地址不能为“滥用”或“邮寄主持人”。


## 委派子域

必须通过创建四个名称服务器(NS)记录来委派选定用于Adobe Campaign平台的子域。  这允许子域正确委派给Adobe。  以下是子域委派和相应DNS说明的示例。  请将“emails.customer.com”替换为要委派的子域。  请注意，子域必须是唯一的，不能由另一方（例如，现有的ESP或MSP）使用。

| 委托子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。  </br> `<subdomain>` NS b.ns.campaign.adobe.com。  </br> `<subdomain>` NS c.ns.campaign.adobe.com。  </br> `<subdomain>` NS d.ns.campaign.adobe.com。 |

## 跟踪、镜像页面、资源

在将电子邮件发送子域正确委派给Adobe Campaign后，Adobe TechOps团队将创建两个或多个较低级别的域来独立管理跟踪和镜像页面。

| 类型 | Domain |
|--- |--- |
| 镜像页面 | m.`<subdomain>` |
| 跟踪 | t.`<subdomain>` |
| 资源 | res.`<subdomain>` |

## 云部署（可选）

仅当Adobe Campaign Classic完全托管在云中时(由Adobe提供)，此情况才适用。  这是可选配置。

任何要开发的调查、Web表单和登陆页均通过完全托管在云中的Adobe Campaign进行管理。  如果需要，可将附加子域委派给Adobe（例如web.customer.com）以用于工具中的任何Web组件。  请注意，子域必须是唯一的，不能由另一方使用（例如，现有的ESP或MSP）。

| 委托子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br>`<subdomain>` NS b.ns.campaign.adobe.com。</br>`<subdomain>` NS c.ns.campaign.adobe.com。</br>`<subdomain>` NS d.ns.campaign.adobe.com。 |

>[!NOTE]
>
>默认情况下，工具中的任何Web组件都将使用委托用于电子邮件的初始子域。

## 云消息部署（可选）

如果Adobe Campaign Classic营销实例由客户事先托管，则客户需要进行其他技术配置。

任何要开发的调查、Web表单和登陆页均通过存在收件人记录的Adobe Campaign营销实例进行管理。

部署由Adobe Campaign营销实例托管的面向外部的Web组件需要其他CNAME DNS配置。  这将允许Internet公开访问Web组件（例如web.customer.com），并使用客户的域添加品牌。

还需要配置防火墙，以允许访问托管这些Web组件的Adobe Campaign营销实例（在端口80或443上）。

**最佳实践Recommendations:**

托管Web组件的子域对客户可见，因此请务必使其正确标记并且记住，因为可能需要手动输入它，例如：https://web.customer.com。
如果需要在安全页面(HTTPS)上托管任何表单，则需要附加技术配置，如下所述。

| 委托子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME  `<internal customer server>` |

## 提供的服务

在这些授权之后，Adobe建立的基础设施确保对每个授权的或基于CNAME的发送域执行以下服务：

* 创建postmaster@和ubuse@收件箱
* 为委托域设置反馈循环
* Adobe还将根据要求配置DMARC记录。 您的可交付性顾问可以协助为您的发送域设计长期DMARC策略和计划。
由Adobe建立的参数仅在完成委派后由Adobe验证时才有效，并且仍然有效。  所有Adobe Campaign Cloud优惠都将域名委派作为标准。

## 计费和实施条件

* 根据初始合同和选定的一揽子类型，除了作为标准列入的代表团之外，还可以包括其他代表团，
* 除这些代表团外，还将收取额外代表团的费用，
* 如初始合同所述，这些额外代表团的账单方法每月额外收费。

如果客户端通过Adobe Campaign工具选择专用于投放的关联域名，并且正确执行了相关文档中详细说明的委托先决条件，则这些委托将被接受。

## 停止服务

在任何时候，客户端都将能够提出书面要求，不再从委托服务中受益，并自行承担必要的DNS配置。

如果出现这种情况，Adobe将向客户端提供详细列出返回非域委派模式所需的服务天数的估计。

如果Adobe未能遵守上述承诺，将免除客户就使用上述可交付性费率承担的任何责任。

Marketing Cloud服务的终止将自动导致域委派的结束，并且这些域的DNS维护将由Adobe进行。

## 使用控制面板监视子域

为实例配置子域后，您可以使用控制面板监视子域。

这允许您视图您委托给Adobe Campaign的所有子域，并请求续订其SSL证书。

有关更多信息，请参阅[专用文档](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates)。

>[!NOTE]
>
>[仅](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html) 对使用Adobe Managed Services的客户提供控制面板。