---
title: 实时黑洞列表
description: 了解维护可能被垃圾邮件发送者使用的IP地址和域列表的组织。
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

一些组织维护着被垃圾邮件发送者使用的IP地址和域的数据库。 查阅这些网站有助于了解为什么某些邮件被作为垃圾邮件拒绝。 通常可以请求移除错误地添加到这些列表中的地址。

这些数据库称为RBL（实时黑洞列表），通过DNS机制来查阅它们。 RBL有三种类型：

* 按IP地址：列出发送垃圾邮件或可能正在转发垃圾邮件的IP地址。
* 按发件人域：列出发送垃圾邮件或配置不正确的发件人域（退回邮件地址的完整域）。
* 按Web域：列出在垃圾邮件内容中包含的链接和图像的URL中找到的域（向注册商注册的高级别域）。 在Adobe解决方案中，要考虑的域通常是用于跟踪的地址。

以下列出最广泛使用的RBL。 有关更全面的列表，您可以参阅 [https://www.dnsstuff.com/](https://tools.dnsstuff.com/).

* **Spamhaus**

  请参阅 [https://www.spamhaus.org/](https://www.spamhaus.org/)

  数据库更重要。 被列入这一名单总的来说是一种严重的情况。 如果发生这种情况，您必须立即采取行动并警告商业服务、可投放性和 [Adobe客户关怀](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html).

* **垃圾邮件**

  请参阅 [https://www.spamcop.net/](https://www.spamcop.net/)

  它是最著名的数据库之一。 如果您的IP地址之一被列入此列表，这通常意味着SpamCop用户已将您的邮件声明为垃圾邮件，或者您已将邮件发送到SpamCop蜜罐。

* **URIBL**

  请参阅 [https://www.uribl.com/](https://www.uribl.com/)

  此列表标识在声明为垃圾邮件的邮件中经常出现的域。 如果您的域出现在此列表中，则会显着影响您的可投放性。 您应告知可投放性服务和 [Adobe客户关怀](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **SURBL**

  请参阅 [https://surbl.org/](https://surbl.org/)

  SURBL标识定期出现在垃圾邮件中的网站。 如果您的域出现在此列表中，则会显着影响您的可投放性。 您应告知可投放性服务和 [Adobe客户关怀](https://helpx.adobe.com/cn/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html) 立即。

* **iX马尼图**

  这是IP列表，在德国被广泛使用。 请参阅 [https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
