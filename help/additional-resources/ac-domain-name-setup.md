---
title: 域名设置
description: 了解如何将子域委派给Adobe Campaign。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 82f7254a9027f79d2af59aece81f032105c192d5
workflow-type: tm+mt
source-wordcount: '2061'
ht-degree: 2%

---

# 域名设置

本文档介绍了域名设置和委派的业务和技术要求。 您需要选择电子邮件发送子域和（可选）面向外部的子域，以托管您正在使用的Adobe平台的Web组件（登陆页、选择退出页）。

>[!NOTE]
>
>您还可以使用控制面板（作为测试版提供）设置新子域。 有关详细信息，请参阅[此部分](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read)。

## 子域

借助Adobe，数字营销可以真正成为上下文引擎，为您的品牌的客户参与营销计划提供支持。  电子邮件仍然是数字营销计划的基础。 但是，到达收件箱变得比以往任何时候都更困难。

为电子邮件促销活动创建子域后，品牌商可以将各种类型的流量（例如营销流量与公司流量）隔离到特定的IP池中并放入特定的域，从而加快 [IP预热过程](../../help/additional-resources/increase-reputation-with-ip-warming.md) 并全面提高可投放性。 如果您共享某个域，但该域被阻止或添加到阻止列表中，则可能会影响您的公司邮件投放。 但是，特定于您的电子邮件营销通信的域上的信誉问题或阻止将仅影响该电子邮件流。  将您的主域用作发件人或多个邮件流的“发件人”地址也可能破坏电子邮件身份验证，导致阻止您的邮件或将其放入垃圾邮件文件夹中。

### 委派

域名委派是一种方法，它允许域名（技术上称为DNS区域）的所有者将其细分（技术上称为DNS区域下的细分）委派给另一个实体。 基本上，如果客户正在处理区域“example.com”，他可以将子区域“marketing.example.com”委派给Adobe Campaign。

这意味着Adobe Campaign的DNS服务器将仅对该区域拥有完全权限，而不会对顶级域拥有完全权限。 Adobe Campaign的DNS服务器将针对该区域中域名的查询提供权威解答，例如“t.marketing.example.com”本身，而不是“www.example.com”。

通过委派子域以用于Adobe Campaign，客户可以依靠Adobe来维护所需的DNS基础架构，以满足其电子邮件营销发送域的行业标准可投放性要求，同时继续维护和控制内部电子邮件域的DNS。  子域委派允许：

客户通过使用带有其域名Adobe的DNS别名来自主实施所有技术最佳实践，以在发送电子邮件期间完全优化投放能力，从而保持其品牌形象

## DNS设置选项

为了提供基于云的托管服务，Adobe强烈建议客户在部署Adobe Campaign时使用子域委派。  但是，Adobe确实为客户端提供了一个用于配置DNS的替代选项 — CNAME设置。

| 选项 | 描述 | Adobe职责 | 客户责任 |
|--- |------- |--- |--- |
| 将子域委派给Adobe Campaign | 客户端将子域(email.example.com)委派给Adobe。 在此方案中，Adobe能够控制和维护传递、渲染和跟踪电子邮件营销活动所需的DNS的所有方面，从而将Campaign作为托管服务提供。 | 完全管理Adobe Campaign所需的子域和所有DNS记录。 | 将子域正确委派给Adobe |
| 使用 CNAME | 客户端创建一个子域，并使用CNAME指向Adobe特定的记录。  使用此设置，Adobe 和客户共同负责维护 DNS。 | 管理Adobe Campaign所需的DNS记录。 | 创建和控制子域，以及创建/管理Adobe Campaign所需的CNAME记录。 |

## 所需的DNS记录

| 记录类型 | 用途 | 示例记录/内容 |
|--- |--- |--- |
| MX | 指定传入邮件的邮件服务器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF (TXT) | 发件人策略框架 | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM (TXT) | 域密钥标识的邮件 | <i>客户。_domainkey.email.example.com</i></br>&quot;v=DKIM1； k=rsa；&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| 托管记录(A) | 镜像页面、图像托管和跟踪链接，所有发送域 | m.email.example.com IN A 123.111.100.99</br>t.email.example.com IN A 123.111.100.98</br>email.example.com IN A 123.111.100.97 |
| 反向DNS (PTR) | 将客户端IP地址映射到客户端标记的主机名 | 18.101.100.192.in-addr.arpa域名指针r18.email.example.com |
| CNAME | 提供另一个域名的别名 | t1.email.example.com是t1.email.example.campaign.adobe.com的别名 |


建议使用基于域的邮件身份验证、报告和符合性(DMARC)来验证邮件发件人的身份，并确保目标电子邮件系统信任从您的域发送的邮件。

DMARC TXT记录示例：

```
_dmarc.email.example.com

“v=DMARC1; p=none; rua=mailto:mailauth-reports@myemail.com” 
```

您可以手动实施DMARC或联系Adobe来帮助您为品牌设置DMARC。

## 设置要求

### 子域委派

这要求客户端在其DNS服务器中创建一个子域，并将此子域的名称服务器定义为Adobe维护的名称服务器。  例如，主域名为“example.com”且想要将“marketing.example.com”的管理委派给Adobe其电子邮件投放的客户，必须具体化此委派才能将以下类型记录添加到其DNS中：

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

委派域名意味着此域将专门用于通过Adobe Campaign平台传送电子邮件，因此不能用于其他方式（例如，从其他电子邮件基础设施发送电子邮件）。

在设置过程中，Adobe将确保该域连接到Adobe传入电子邮件基础架构，以便管理和处理返回这些域的回弹电子邮件（MX类型DNS记录配置）。

### 使用 CNAME

如果客户端选择使用CNAME而不是将子域委派给Adobe，则在设置阶段，Adobe将提供要放置在客户端DNS服务器中的记录，并将在Adobe Campaign DNS服务器中配置相应的值。

## 部署的一般要求

实施新的企业营销解决方案时，需要面向外部的组件。  这些功能包括托管登陆页面和Web窗体，设置要跟踪的链接和网页，显示镜像页面，以及配置选择退出页面。

虽然这些要求通过Adobe和客户托管的组件进行管理，但它们包含的URL可供电子邮件的收件人查看。  为避免具有指示底层技术解决方案或托管提供商的URL，可设置子域以使其对电子邮件的收件人透明。  例如，在查看http://www.customer.com/等URL时，该域将为“www.customer.com”。  其子域将为“www”。

### 子域要求

确定要用于Adobe Campaign应用程序中的品牌URL（镜像页面和跟踪URL）的子域。  此外，还要确定电子邮件投放中每个子域的“发件人地址”、“发件人姓名”和“回复地址”是什么。

完成下表，第一行只是一个示例。

| Subdomain | 发件人地址 | 发件人姓名 | 回复地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Customer | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* “回复地址”字段的用途是您希望收件人回复的地址与“发件人地址”不同。  虽然不是必填字段，但Adobe强烈建议“回复地址”有效并链接到受监视邮箱。  此邮箱必须由客户托管。  它可以是支持邮箱，例如customercare@customer.com，在其中读取和响应电子邮件。
>* 如果客户未选择“回复地址”，则默认地址始终为 `<tenant>-<type>-<env>@<subdomain>`.
>* 当以此方式设置“回复地址”时，回复将发送到不受监视的邮箱。
>* 从Adobe Campaign发送电子邮件时，不会监控“发件人地址”邮箱，并且营销用户无法访问此邮箱。 Adobe Campaign也不提供自动回复或自动转发此邮箱中接收的电子邮件的功能。
>* 促销活动发件人/发件人地址和错误地址不能为“滥用”或“邮递员”。

## 委派子域

必须通过创建四个名称服务器(NS)记录来委派选定用于Adobe Campaign平台的子域。  这允许将子域正确委派给Adobe。  以下是子域委派和相应DNS说明的示例。  请将“emails.customer.com”替换为要委派的子域。  请注意，子域必须是唯一的，并且不能被另一方（例如，现有ESP或MSP）使用。

| 已委派的子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com. </br> `<subdomain>` NS b.ns.campaign.adobe.com. </br> `<subdomain>` NS c.ns.campaign.adobe.com. </br> `<subdomain>` NS d.ns.campaign.adobe.com. |

## 跟踪、镜像页面、资源

将电子邮件发送子域正确委派给Adobe Campaign后，AdobeTechOps团队将创建两个或多个较低级别的域，以独立管理跟踪和镜像页面。

| 类型 | 域 |
|--- |--- |
| 镜像页面 | m.`<subdomain>` |
| 跟踪 | t.`<subdomain>` |
| 资源 | 解析`<subdomain>` |

## 云部署（可选）

仅当Adobe Campaign Classic由Adobe完全托管在云中时，这才适用。  这是一个可选配置。

要开发的任何调查、Web窗体及登陆页面都通过Adobe Campaign进行管理，并在云中完全托管。  如果需要，可以将其他子域委派给Adobe(例如web.customer.com)，以用于工具中的任何Web组件。  请注意，子域必须是唯一的，不能由其他方（例如，现有的ESP或MSP）使用。

| 已委派的子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com.</br>`<subdomain>` NS b.ns.campaign.adobe.com.</br>`<subdomain>` NS c.ns.campaign.adobe.com.</br>`<subdomain>` NS d.ns.campaign.adobe.com. |

>[!NOTE]
>
>默认情况下，该工具中的任何Web组件都将使用委派用于电子邮件的初始子域。

## 云消息部署（可选）

如果Adobe Campaign Classic营销实例托管在客户本地，则客户需要执行其他技术配置。

任何要开发的调查、Web窗体以及登陆页都通过Adobe Campaign营销实例进行管理，该实例中存在收件人记录。

部署由Adobe Campaign营销实例托管的面向外部的Web组件需要其他CNAME DNS配置。  这将允许Internet公开访问Web组件(例如，web.customer.com)，并标记客户的域。

还需要将防火墙配置为允许访问托管这些Web组件的Adobe Campaign营销实例（位于端口80或443）。

**Recommendations最佳实践：**

客户将能够看到托管Web组件的子域，因此请确保正确标记该子域并易于记忆，因为可能需要手动键入该子域，例如：https://web.customer.com。
如果需要将任何表单托管在安全页面(HTTPS)上，则需要额外的技术配置，如下所述。

| 已委派的子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME `<internal customer server>` |

## 提供的服务

在这些委托之后，Adobe建立的基础架构确保为每个委托的或带有CNAME别名的发送域执行以下服务：

* 创建邮局管理员@和滥用@收件箱
* 为委派域设置反馈循环
* 根据请求，Adobe还将按照指定配置DMARC记录。 您的可投放性顾问可以协助您设计长期的DMARC策略并为发送域制定计划。
由Adobe建立的参数仅在委派完成并由Adobe验证后有效，并且仍然有效。  所有Adobe Campaign Cloud选件都包括作为标准的域名委派。

## 计费和实施条件

* 根据初步合同和所选一揽子方案的类型，除了作为标准列入初步授权之外的其他授权也可包括在内，
* 除这些包括的委派外，还将向其他委派收费，
* 这些额外委托的计费方式是按最初合同规定每月额外付费。

这些委派将被接受，前提是客户端选择通过Adobe Campaign工具专用于投放的相关域名，并且正确实施相关文档中详述的委派先决条件。

## 服务中断

在任何时候，客户端都将能够提出书面要求，要求不再从委派服务中受益，并且自己承担必要的DNS配置。

如果发生这种情况，Adobe将为客户端提供一个估计值，详细说明返回非域委派模式所需的服务天数。

倘客户未能遵守上述承诺，则就使用上述可交付性比率所承担之任何责任将Adobe解除。

终止Marketing Cloud服务将自动导致域委派的结束，并按Adobe对这些域进行DNS维护。

## 使用控制面板监控子域

为您的实例配置子域后，您可以使用控制面板监控它们。

这样，您就可以查看委派给Adobe Campaign的所有子域，并请求续订其SSL证书。

有关更多信息，请参阅[专用文档](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates)。

>[!NOTE]
>
>[控制面板](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html) 仅适用于使用AdobeManaged Services的客户。
