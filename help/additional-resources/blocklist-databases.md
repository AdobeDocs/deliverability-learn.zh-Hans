---
title: 实时黑洞列表
description: 了解维护IP地址列表以及可能由垃圾邮件发送者使用的域的组织。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: 4155b89f-a636-404c-8951-563c1b4d0289
source-git-commit: e7427d6109f3201affa58decde36294d1631bf5b
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 6%

---

# 实时黑洞列表

一些组织维护据称由垃圾邮件发送者使用的IP地址和域的数据库。 咨询这些网站有助于了解为什么某些邮件被拒绝为垃圾邮件。 通常可以请求删除错误地添加到这些列表的地址。

这些数据库称为RBL（实时黑洞列表），并通过DNS机制查询它们。 RBL有三种类型：

* 按IP地址：列出发送垃圾邮件或可能中继垃圾邮件的IP地址。
* 按发件人域：列出发送垃圾邮件或配置不正确的发件人域（退回邮件地址的完整域）。
* 按Web域：列出在垃圾邮件内容中包含的链接和图像的URL中找到的域（在注册商处注册的高级域）。 在Adobe解决方案中，要考虑的域通常是用于跟踪的地址。

以下是使用最广泛的RBL的列表。 有关更全面的列表，您可以参阅 [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **斯帕姆豪斯**

   请参阅 [https://www.spamhaus.org/](https://www.spamhaus.org/)

   数据库更重要。 被列入这一名单通常是严重的情况。 如果发生这种情况，您必须立即采取行动，并警告商业服务、投放能力和 [Adobe客户关怀](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **SpamCop**

   请参阅 [https://www.spamcop.net/](https://www.spamcop.net/)

   它是最著名的数据库之一。 如果您的某个IP地址被置于此列表中，这通常意味着SpamCop用户已将您的消息声明为垃圾邮件，或您已将消息发送到SpamCop蜜罐。

* **URIBL**

   请参阅 [https://www.uribl.com/](https://www.uribl.com/)

   此列表标识定期显示在声明为垃圾邮件的邮件中的域。 如果您的域显示在此列表中，则可能会显着影响您的投放能力。 您应通知可投放性服务和 [Adobe客户关怀](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **SURBL**

   请参阅 [https://surbl.org/](https://surbl.org/)

   SURBL识别定期显示在垃圾邮件中的网站。 如果您的域显示在此列表中，则可能会显着影响您的投放能力。 您应通知可投放性服务和 [Adobe客户关怀](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **iX曼尼图**

   这是一系列知识产权，在德国被广泛使用。 请参阅 [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
