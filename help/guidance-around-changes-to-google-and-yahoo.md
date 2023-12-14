---
title: 有关宣布的更改的指导，请参见 [!DNL Google] 和 [!DNL Yahoo]
description: 有关宣布的更改的指导，请参见 [!DNL Google] 和 [!DNL Yahoo]
role: Admin
level: Experienced
doc-type: Article
last-substantial-update: 2023-11-06T00:00:00Z
jira: KT-14320
thumbnail: KT-14320.jpeg
exl-id: 879e9124-3cfe-4d85-a7d1-64ceb914a460
source-git-commit: 2bda5d5369d239fac849e57286450a853dd94953
workflow-type: tm+mt
source-wordcount: '1605'
ht-degree: 0%

---

# 有关宣布的更改的指导，请参见 [!DNL Google] 和 [!DNL Yahoo]

10月3日，两者 [!DNL Google] 和 [!DNL Yahoo] 通过他们的博客联合宣布了计划的更改。 这些更改旨在从2024年2月1日开始强制实施一些常见的行业最佳做法作为新要求，从而使其用户更安全，并提供更好的电子邮件体验。

[https://blog.google/products/gmail/gmail-security-authentication-spam-protection/](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/){target="_blank"}

![[!DNL Google] 公告_](/help/assets/Gmail.png)

[https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam](https://blog.postmaster.yahooinc.com/post/730172167494483968/more-secure-less-spam){target="_blank"}

![[!DNL Yahoo] 公告](/help/assets/Yahoo.png)

Adobe的电子邮件可投放性专家已阅读这些博客文章和所有链接文档，因此您不必这样做。 以下是主要要点。

## 那么，到底是什么呢？ [!DNL Google] 和 [!DNL Yahoo] 在干吗？

在电子邮件领域中，存在着法律要求、实际要求和一般最佳实践。 法律要求因地点不同而大不相同，不属于本主题。 相反， [!DNL Google] 和 [!DNL Yahoo] 正在采用最佳实践，并将其转变为实际要求。

无任何项目 [!DNL Google] 和 [!DNL Yahoo] 2月开始的要求是新的，多年来一直是最佳实践建议，但行业采用速度缓慢且参差不齐。 这是 [!DNL Google] 和 [!DNL Yahoo]，这是通过以下方式帮助推进采纳流程的方法：声明“如果您希望将电子邮件部署到我们的用户（这占电子邮件列表的一大部分，在某些情况下高达70%，具体取决于地区和行业），您需要完成这些任务。”

## 具体情况如何？

如果您是Adobe客户，他们的大多数要求都是您的设置的一部分，但有一些关键项目在技术上是可选的，您希望确保贵组织在2024年2月之前已正确采用和设置这些项目。

## DMARC：

[!DNL Google] 和 [!DNL Yahoo] 都将要求您拥有DMARC记录，才能访问您用来向其发送电子邮件的任何域。 它们当前不要求p=reject或p=quarantine设置，因此p=none（通常称为“监控”设置）设置是完全可接受的。 这不会改变您电子邮件的处理方式，它们将执行不使用DMARC时通常会执行的操作。 设置此设置是使用DMARC保护自己的第一步，也是帮助您将电子邮件发送到的新好处 [!DNL Google] 和 [!DNL Yahoo] 它还可以帮助您查看电子邮件生态系统中的任何位置是否存在身份验证问题。

DMARC的规则不会更改，这意味着除非配置为阻止它，否则父域上的DMARC记录(例如adobe.com)将被继承并涵盖任何子域(例如email.adobe.com)。 您不需要为子域添加不同的DMARC记录，除非您出于各种业务原因需要添加它们。

目前Adobe完全支持DMARC，但不是必需的。 使用任何免费的DMARC检查器查看您的子域是否设置了DMARC，如果不设置，请与Adobe支持团队联系，了解如何最好地进行该设置。

您还可以找到有关DMARC及其实施方法的更多信息 [此处](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/technotes/implement-dmarc.html?lang=zh-Hans){target="_blank"} for Adobe Campaign or [here](https://experienceleague.adobe.com/docs/marketo/using/getting-started-with-marketo/setup/configure-protocols-for-marketo.html){target="_blank"} 用于Marketo Engage。

## 一键单击（列表）取消订阅：

不要惊慌。 [!DNL Google] 和 [!DNL Yahoo] 不是指您的电子邮件正文或页脚中的取消订阅链接，这些链接可能会被安全机器人在执行任务时点击，或者被意外点击。 这意味着“mailto”或“http/URL”版本的List-Unsubscribe标头功能。 此函数位于 [!DNL Yahoo] 和Gmail UI，用户可以单击取消订阅。 Gmail甚至会提示单击“报告垃圾邮件”的用户查看他们是否想要取消订阅，这可以通过将他们变为取消订阅来减少您收到的投诉数量（投诉会损害您的声誉）（不会损害您的声誉）。

请务必注意 [!DNL Google] 和 [!DNL Yahoo] 均以名称“1-Click”引用“http/URL”选项，这是有意为之。 从技术上讲，最初的“http/URL”选项允许您将收件人重定向到网站。 这不是 [!DNL Yahoo] 和 [!DNL Google]，均引用已更新的 [RFC8058](https://datatracker.ietf.org/doc/html/rfc8058){target="_blank"} ，重点是通过HTTPSPOST请求（而不是网站）处理取消订阅，使其成为“一键式”请求。

今天，Gmail接受“mailto”列表取消订阅选项。 Gmail表示，“mailto”无法满足他们未来的期望，发件人需要启用“发布”列表取消订阅选项。 在2024年6月1日之前，已设置某种类型的列表取消订阅的发件人必须实施“1键单击”列表取消订阅。

[!DNL Yahoo] 已经表示，他们目前将继续遵守“邮寄”选项，但将来也会要求“邮寄”。

Adobe建议同时使用“mailto”和“post/1-Click”列表取消订阅选项。 Adobe正在努力启用所有电子邮件发送平台上的“发布”支持，以支持用户满足这些要求，并为此目的进行进一步的更新。

对于Marketo Engage，Adobe已启用“mailto”选项，当前不支持“http/URL”选项。 关于此功能的进一步更新。
对于Adobe Campaign和Adobe Journey Optimizer，Adobe建议同时使用“mailto”和“1-Click”选项。

对列表取消订阅标头的需求不适用于事务性电子邮件。 请注意，触发的消息（如放弃的购物车和订阅者未生成的类似通信）被视为邮箱提供商的营销消息，例如 [!DNL Google] 和 [!DNL Yahoo] 这些需要取消列表订阅。

[!DNL Google] 和 [!DNL Yahoo] 都知道在某些情况下，收件人将取消订阅，然后稍后重新订阅。 虽然他们不愿意分享他们如何识别这些情况的秘密秘密，但他们正在努力寻找方法，避免在这些情况下错误地惩罚发件人。

>[!INFO]
> 有关如何为解决方案实施list-unsubscribe的更多信息，请查看：
> * [!DNL Adobe Campaign Classic]： [技术建议](https://experienceleague.adobe.com/docs/deliverability-learn/deliverability-best-practice-guide/additional-resources/campaign/acc-technical-recommendations.html?lang=en#list-unsubscribe){target="_blank"}
>* [!DNL Adobe Campaign Standard]： [什么是List-Unsubscribe标头？ 如何在ACS中实现这一点？](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-14778.html?lang=en){target="_blank"}
>* [!DNL Adobe Journey Optimizer]： [电子邮件选择退出管理](https://experienceleague.adobe.com/docs/journey-optimizer/using/email/email-opt-out.html?lang=en){target="_blank"}
>
> 或随时联系Adobe客户支持团队。


## 在2天内取消订阅进程：

我们推荐此最佳实践一段时间以来，因为您部署到已取消订阅的人的每封电子邮件通常都会导致垃圾邮件投诉，因此越快停止向他们发送电子邮件越好。 同样，在某些情况下，法律要求可能要长得多，但是 [!DNL Google] 和 [!DNL Yahoo] 将知道其用户已通过List-Unsubscribe取消订阅，并且您在第3天仍向他们发送电子邮件，并且他们已声明，不允许进行此类操作的发件人继续向其任何用户发送电子邮件。
对于通过各种list-unsubscribe方法取消订阅的任何内容，此为期2天的要求如下： 在某些情况下（例如“mailto”），这意味着Adobe将处理这些事件。 Adobe在收到请求后立即处理所有取消订阅请求，并且不超过2天。 在其他情况下，您可能会处理这些请求。 如果您正在处理这些请求，则可能需要做出更改以符合这个2天的时间表。

## 投诉率：

长久以来，将低投诉率保持在0.2%以下一直是一种最佳做法。 [!DNL Google] 将长期保持在0.3%的水平上调，但明确表示将这一标准保持在0.1%以下是有好处的。 以下是他们共享的详细信息：

* 将垃圾邮件率保持在0.10%以下。
* 避免0.30%或更高的垃圾邮件率，尤其是对于任何持续的时间段。
* 保持较低的垃圾邮件率使发件人能够更好地抵御用户反馈的偶尔激增。
* 同样，保持高垃圾邮件率将导致垃圾邮件分类增加。 改进垃圾邮件率可能需要一段时间才能对垃圾邮件分类产生积极影响。
  [!DNL Yahoo] 已表示他们的投诉阈值也将在0.30%的范围内。

[!DNL Google] 和 [!DNL Yahoo]他们的目标不是因为某一天的错误或导致投诉暂时激增的错误而惩罚发件人。 相反，他们关注的是长期投诉率高或发送行为不好的发件人。

如果您在监控投诉率方面需要帮助，或者希望帮助制定减少投诉的策略，请咨询您的Adobe可投放性顾问，或者与您的客户团队讨论添加可投放性顾问（如果您还没有这样的顾问）。

## 这对我作为营销人员有何影响？

未能遵守Gmail和 [!DNL Yahoo] 可能导致电子邮件登录垃圾邮件文件夹或被阻止（即，从MBP返回以指示电子邮件未投放）。

因此，Adobe强烈建议您执行上述更改，并确保尽快开始遵守这些更改。 现在也是开始在以下网站设定绩效基准的绝佳时机： [!DNL Yahoo] 和 [!DNL Google] 以便您查看在明年2月之后您的量度是否有任何实质性更改。

我们随时为您提供帮助，因此如果您有任何问题或需要支持，请咨询您的Adobe可交付性顾问，或者与您的客户团队讨论添加可交付性顾问（如果您还没有顾问）。

## 有办法解决这个问题吗？

虽然这个问题总是会出现，但现实情况是，这些更改对的最终用户是有意义的 [!DNL Google] 和 [!DNL Yahoo]的平台。 这些用户希望执行这些规则，这激励了他们。 我们不建议尝试绕过任何这些更改，而是应该后退一步，考虑如何适应这些更改。

## 最终说明：

请注意，这当前不适用于发送给的电子邮件 [!DNL Yahoo].JP或 [!DNL Gmail] 但是，它适用于来自这些位置的电子邮件。

## 其他资源（并非特定于这些更改）：

[Google发件人准则](https://support.google.com/mail/answer/81126){target="_blank"}

[Google常见问题解答](https://support.google.com/a/answer/14229414?sjid=2864589551334481470-NC){target="_blank"}

[Yahoo发件人准则](https://senders.yahooinc.com/best-practices/){target="_blank"}

[Yahoo常见问题解答](https://senders.yahooinc.com/faqs/){target="_blank"}
