---
title: 域名设置
description: 了解如何将子域委派给Adobe Campaign。
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
exl-id: 4d52d197-d20e-450c-bfcf-e4541c474be4
source-git-commit: 68c403f915287e1a50cd276b67b3f48202f45446
workflow-type: tm+mt
source-wordcount: '2028'
ht-degree: 2%

---

# 域名设置

本文档介绍了域名设置和委派的业务和技术要求。 您将需要选择电子邮件发送子域，以及（可选）面向外部的子域，以托管您所使用的Adobe平台的Web组件（登陆页面、选择退出页面）。

>[!NOTE]
>
>您还可以使用控制面板设置新子域（可作为测试版使用）。 在[此部分](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/setting-up-new-subdomain.html#must-read)中了解详情。

## 子域

借助Adobe，数字营销可以真正成为推动您品牌的客户参与营销计划的情境引擎。  电子邮件仍是数字营销计划的基础。 但是，进入收件箱比以往更加困难。

通过为电子邮件促销活动创建子域，品牌可以将不同类型的流量（例如，营销与公司流量）隔离到特定IP池中，并使用特定的域，从而加快[IP预热过程](../../help/additional-resources/increase-reputation-with-ip-warming.md)并提高整体投放能力。 如果您共享域，并且该域被阻止或添加到阻止列表中，则可能会影响您的公司邮件投放。 但是，特定于电子邮件营销通信的域名存在的声誉问题或阻止，将仅影响电子邮件的流量。  如果将主域用作多个邮件流的发件人或“发件人”地址，则也可能会中断电子邮件身份验证，从而阻止您的邮件或将其置于垃圾邮件文件夹中。

### 委派

域名委派是允许域名所有者的方法(技术上为：DNS区域)，以委派其分区(技术上：其下的DNS区域（可称为子区域）到其他实体。 基本上，如果客户正在处理区域“example.com”，则可以将子区域“marketing.example.com”委派给Adobe Campaign。

这意味着Adobe Campaign的DNS服务器将仅对该区域拥有完全权限，而不对顶级域拥有完全权限。 Adobe Campaign的DNS服务器将对该区域中域名(例如“t.marketing.example.com”本身，但不是“www.example.com”)的查询提供权威性答案。

通过委派子域以与Adobe Campaign一起使用，客户可以依赖Adobe来维护满足其电子邮件营销发送域行业标准可交付性要求所需的DNS基础结构，同时继续维护和控制其内部电子邮件域的DNS。  子域委派允许：

客户使用带有其域名的DNS别名来保留其品牌形象
Adobe自主实施所有技术最佳实践，以在发送电子邮件期间充分优化投放能力

## DNS设置选项

为了提供基于云的托管服务，Adobe强烈鼓励客户在部署Adobe Campaign时使用子域委派。  但是，Adobe确实为客户端提供了配置DNS的替代选项 — CNAME设置。

| 选项 | 描述 | Adobe责任 | 客户责任 |
|--- |------- |--- |--- |
| 将子域委派到Adobe Campaign | 客户端将子域(email.example.com)委派给Adobe。 在此方案中，Adobe能够控制和维护发送、渲染和跟踪电子邮件促销活动所需的DNS的所有方面，从而将促销活动作为托管服务进行交付。 | 完全管理子域和Adobe Campaign所需的所有DNS记录。 | 正确委派子域以Adobe |
| 使用 CNAME | 客户端会创建子域，并使用CNAME指向特定于Adobe的记录。  使用此设置，Adobe 和客户共同负责维护 DNS。 | 管理Adobe Campaign所需的DNS记录。 | 创建和控制子域，以及创建/管理Adobe Campaign所需的CNAME记录。 |

## 所需的DNS记录

| 记录类型 | 用途 | 示例记录/内容 |
|--- |--- |--- |
| MX | 为传入邮件指定邮件服务器 | <i>email.example.com</i></br><i>10 inbound.email.example.com</i> |
| SPF(TXT) | 发件人策略框架 | <i>email.example.com</i></br>&quot;v=spf1 redirect=__spf.campaign.adobe.com&quot; |
| DKIM(TXT) | 已识别的DomainKeys邮件 | <i>客户。_domainkey.email.example.com</i></br>&quot;v=DKIM1;k=rsa;&quot; &quot;DKIMPUBLICKEY HERE&quot; |
| DMARC(TXT) | 基于域的消息身份验证 | 报告与合规性 | _dmarc.email.example.com</br>&quot;v=DMARC1;p=none;rua=mailto:mailauth-reports@myemail.com |
| 主机记录(A) | 镜像页面、图像托管和跟踪链接，所有发送域 | m.email.example.com A 123.111.100.99</br>t.email.example.com A 123.111.100.98</br>email.example.com A 123.111.100.97 |
| 反向DNS(PTR) | 将客户端IP地址映射到客户端品牌主机名 | 18.101.100.192.in-addr.arpa域名指针r18.email.example.com |
| CNAME | 为其他域名提供别名 | t1.email.example.com是 | t1.email.example.campaign.adobe.com |

## 设置要求

### 子域委派

这要求客户端在其DNS服务器中创建子域，并将此子域的名称服务器定义为由Adobe维护的子域。  例如，如果客户的主域名为“example.com”，并且希望将“marketing.example.com”的管理委派给Adobe以进行电子邮件投放，则必须实体化此委派，以将以下类型记录添加到其DNS:

```
marketing.example.com. NS a.ns.campaign.adobe.com.
marketing.example.com. NS b.ns.campaign.adobe.com.
marketing.example.com. NS c.ns.campaign.adobe.com.
marketing.example.com. NS d.ns.campaign.adobe.com.
```

委派域名意味着此域将专门用于通过Adobe Campaign平台发送电子邮件，因此不能用于其他方式（例如，从其他电子邮件基础结构发送电子邮件）。

在设置过程中，Adobe将确保将域附加到Adobe传入的电子邮件基础结构，以便管理和处理返回到这些域（MX类型DNS记录配置）的反弹电子邮件。

### 使用 CNAME

如果客户端选择使用CNAME而不是委派子域进行Adobe，则在设置阶段，Adobe将提供要置于客户端DNS服务器中的记录，并将在Adobe Campaign DNS服务器中配置相应的值。

## 部署的一般要求

实施新的企业营销解决方案时，需要面向外部的组件。  这些功能包括托管登陆页面和Web窗体、设置要跟踪的链接和网页、显示镜像页面以及配置选择退出页面。

虽然这些要求通过由Adobe和客户托管的组件进行管理，但它们包含可供电子邮件收件人查看的URL。  为避免拥有指示底层技术解决方案或托管提供商的URL，可以设置子域以使其对电子邮件的收件人透明。  例如，在查看http://www.customer.com/之类的URL时，域将为“www.customer.com”。  此的子域将为“www”。

### 子域要求

确定要用于Adobe Campaign应用程序中的品牌URL（镜像页面和跟踪URL）的子域。  此外，还决定电子邮件投放中每个子域的“发件人地址”、“发件人名称”和“回复地址”。

填写下表，第一行仅是一个示例。

| Subdomain | 发件人地址 | 从名称 | 回复地址 |
|--- |--- |--- |--- |
| emails.customer.com | news@emails.customer.com | Customer | customercare@customer.com |
| </br> | </br> | </br> | </br> |

>[!NOTE]
>
>* “回复地址”字段的用途是希望收件人回复与“发件人地址”不同的地址。  虽然不是必填字段，但Adobe强烈建议“回复地址”有效并链接到受监控的邮箱。  此邮箱必须由客户托管。  它可以是一个支持邮箱，例如customercare@customer.com，用于读取和响应电子邮件。
>* 如果客户未选择“Reply-To Address”，则默认地址始终为`<tenant>-<type>-<env>@<subdomain>`。
>* 以这种方式设置“回复地址”时，将向未监控的邮箱发送回复。
>* 从Adobe Campaign发送电子邮件时，“发件人地址”邮箱不受监控，营销用户无法访问此邮箱。 Adobe Campaign还不提供自动回复或自动转发此邮箱中收到的电子邮件的功能。
>* 营销活动发件人/发件人地址和错误地址不能为“滥用”或“邮递员”。


## 委派子域

必须通过创建四个名称服务器(NS)记录来委派选定用于Adobe Campaign平台的子域。  这样可将子域正确委派给Adobe。  以下是子域委派和相应DNS说明的示例。  请将“emails.customer.com”替换为您要委派的子域。  请注意，子域必须是唯一的，且不能已被其他方（例如，现有的ESP或MSP）使用。

| 委派子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。  </br> `<subdomain>` NS b.ns.campaign.adobe.com。  </br> `<subdomain>` NS c.ns.campaign.adobe.com。  </br> `<subdomain>` NS d.ns.campaign.adobe.com。 |

## 跟踪、镜像页面、资源

将电子邮件发送子域正确委派给Adobe Campaign后，Adobe技术运营团队将创建两个或多个较低级别的域，以独立管理跟踪和镜像页面。

| 类型 | Domain |
|--- |--- |
| 镜像页面 | m.`<subdomain>` |
| 跟踪 | t.`<subdomain>` |
| 资源 | res.`<subdomain>` |

## 云部署（可选）

仅当Adobe Campaign Classic由Adobe完全托管在云中时，才适用此方法。  这是一个可选配置。

任何要开发的调查、Web窗体和登陆页面均通过云中完全托管的Adobe Campaign进行管理。  如果需要，可将其他子域委派给Adobe（例如web.customer.com），以用于工具中的任何Web组件。  请注意，子域必须是唯一的，不能被其他方（例如，现有的ESP或MSP）使用。

| 委派子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` NS a.ns.campaign.adobe.com。</br>`<subdomain>` NS b.ns.campaign.adobe.com。</br>`<subdomain>` NS c.ns.campaign.adobe.com。</br>`<subdomain>` NS d.ns.campaign.adobe.com。 |

>[!NOTE]
>
>默认情况下，工具中的任何Web组件都将使用委派用于电子邮件的初始子域。

## 云消息传送部署（可选）

如果Adobe Campaign Classic营销实例在客户的内部部署中托管，则客户将需要进行其他技术配置。

任何要开发的调查、Web窗体和登陆页面均通过收件人记录所在的Adobe Campaign营销实例进行管理。

部署由Adobe Campaign营销实例托管的面向外部的Web组件时，需要额外的CNAME DNS配置。  这将允许Web组件（例如web.customer.com）在Internet上公开访问，并使用客户的域进行标记。

还需要配置防火墙，以允许访问托管这些Web组件的Adobe Campaign营销实例（在端口80或443上）。

**最佳实践Recommendations:**

托管Web组件的子域将对客户可见，因此请确保该子域具有正确的品牌，并且易于记住，因为可能需要手动键入该子域，例如：https://web.customer.com。
如果需要在安全页面(HTTPS)上托管任何表单，则需要添加技术配置，如下所述。

| 委派子域 | DNS说明 |
|--- |--- |
| `<subdomain>` | `<subdomain>` CNAME  `<internal customer server>` |

## 已提供的服务

在执行这些授权后，由Adobe建立的基础架构可确保为每个授权的或基于CNAME的发送域执行以下服务：

* 在收件箱中创建postmaster@和ubuse@
* 为委派的域设置反馈循环
* Adobe还将根据要求配置DMARC记录。 您的可交付性顾问可以协助设计长期的DMARC策略并规划您的发送域。
由Adobe建立的参数仅在委派完成后由Adobe验证并保持正常工作时有效。  所有Adobe Campaign Cloud选件都包括作为标准的域名委派。

## 计费和实施条件

* 根据初始合同和选定的一揽子类型，除了作为标准而列入的代表团之外，还可列入其他代表团，
* 除这些代表团外，还将开具额外的代表团费用，
* 这些额外委托的计费方法按初始合同规定的每月额外收费。

如果客户端选择通过Adobe Campaign工具投放的专用关联域名，并且正确实施了相关文档中详细描述的委派先决条件，则将接受这些委派。

## 停止服务

客户端随时能够提出书面要求，不再从委派服务中受益，并自行进行必要的DNS配置。

如果应该出现这种情况，Adobe将向客户端提供一项估计，详细说明返回到非域委派模式所需的服务天数。

如Adobe未能遵守上述承诺，则客户将不再就参与上述可交付性比率承担任何责任。

终止Marketing Cloud服务将自动导致域委派结束，并按Adobe对这些域进行DNS维护。

## 使用控制面板监控子域

为实例配置子域后，您可以使用控制面板监控子域。

这允许您查看您委派给Adobe Campaign的所有子域，以及请求续订其SSL证书。

有关更多信息，请参阅[专用文档](https://experienceleague.adobe.com/docs/control-panel/using/subdomains-and-certificates/monitoring-subdomains.html#subdomains-and-certificates)。

>[!NOTE]
>
>[控制](https://experienceleague.adobe.com/docs/control-panel/using/control-panel-home.html?lang=zh-Hans) 面板仅适用于使用Adobe Managed Services的客户。
