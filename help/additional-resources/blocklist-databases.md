---
title: 实时黑洞列表
description: 了解维护IP地址和可能被垃圾邮件发送者使用的域的列表的组织。
feature: Additional resources
topics: Deliverability
kt: null
thumbnail: null
doc-type: article
activity: understand
team: ACS
translation-type: tm+mt
source-git-commit: 96ed84da391faaabd3001ddd6a411ddc1f46b033
workflow-type: tm+mt
source-wordcount: '409'
ht-degree: 0%

---


# 实时黑洞列表

一些组织维护据称被垃圾邮件发送者使用的IP地址和域数据库。 咨询这些网站有助于了解为什么某些邮件被拒绝为垃圾邮件。 通常可以请求删除错误地添加到这些列表的地址。

这些列表库称为RBL（实时黑洞数据库），并通过DNS机制查询。 RBL有三种类型：

* 按IP地址：列表发送垃圾邮件或可能转发垃圾邮件的IP地址。
* 按发件人域：列表发送垃圾邮件或配置有误的发件人域（弹回邮件地址的完整域）。
* 按Web域：列表在垃圾邮件内容中包含的链接和图像的URL中找到的域（在注册商处注册的高级域）。 在Adobe解决方案中，要考虑的域通常是用于跟踪的地址。

以下是使用最广的RBL的列表。 有关更全面的列表，请参阅[https://www.dnsstuff.com/](https://tools.dnsstuff.com/)。

* **斯帕姆豪斯**

   请参阅[https://www.spamhaus.org/](https://www.spamhaus.org/)

   数据库更重要。 在这一列表被分类通常是一种严重的情况。 如果发生这种情况，您必须立即采取行动并警告商业服务、可交付性和[Adobe客户关怀](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **SpamCop**

   请参阅[https://www.spamcop.net/](https://www.spamcop.net/)

   它是最著名的数据库之一。 如果您的IP地址之一放置在此列表上，这通常意味着SpamCop用户已将您的邮件声明为垃圾邮件，或者您已将邮件发送到SpamCop蜜罐。

* **URIBL**

   请参阅[https://www.uribl.com/](https://www.uribl.com/)

   此列表标识在声明为垃圾邮件的邮件中经常显示的域。 如果您的域出现在此列表上，可能会显着影响您的可交付性。 您应立即通知交付服务和[Adobe客户关怀](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **SURBL**

   请参阅[http://www.surbl.org/](http://www.surbl.org/)

   SURBL可识别定期出现在垃圾邮件中的网站。 如果您的域出现在此列表上，可能会显着影响您的可交付性。 您应立即通知交付服务和[Adobe客户关怀](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/support-for-experience-cloud.ug.html)。

* **iX Manitu**

   这是知识产权的列表，在德国被广泛使用。 请参阅[https://www.heise.de/ix/nixspam/](https://www.heise.de/ix/nixspam/)

<!--* SORBS

  [https://www.nl.sorbs.net](https://www.nl.sorbs.net) compiles a list of IP addresses that are reputed to be dynamic IP address (i.e. attributed temporarily to ISP subscribers) or "open relay" addresses. Certain domains check whether the IP address of a sender is not listed on this site before accepting email. Checking the IP addresses on this site can prove useful.-->
