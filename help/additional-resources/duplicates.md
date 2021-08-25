---
title: 重复
description: 了解如何识别和限制重复项以提高投放能力。
topics: Deliverability
doc-type: article
activity: understand
team: ACS
exl-id: f89dbb38-a8d4-4294-b017-6fed72591593
source-git-commit: d6094cd2ef0a8a7741e7d8aa4db15499fad08f90
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 6%

---

# 重复 {#duplicates}

拥有重复的电子邮件地址可能会产生多种后果：

* 多次发送同一消息。 即使Adobe在发送之前默认执行重复数据删除过程，在拆分目标时，也不会阻止具有相同内容的不同操作发送相同消息。
* 不接受退订请求。 如果收件人在收到消息后取消订阅，则其重复的用户档案仍符合将来消息的条件。

除了选择加入过程的这一侧步之外，这种情况还可能导致用户将消息视为垃圾邮件，并在ISP触阻止列表发过程。

对数据库执行操作时，必须特别谨慎：

* 必须仔细配置导入，特别是在选择协调键值时。
* 更改的电子邮件地址也可以是重复的源。 特别是，具有不同域的两个地址可能会被路由到同一邮箱，例如，如果公司更改了名称并维护了前一个域达到一定时间：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自动导入（无论是列表还是来自其他数据库）是管理用户档案时要考虑的元素。 删除或移动另一个分区中的配置文件时会发生什么情况？ 例如，在下订单时，可通过自动导入在初始分区中重新创建该分区。
* 可以使用视图而不是分区来实施将配置文件存储在不同文件夹中。 这样，您就可以确保配置文件位于同一物理分区中，同时仍然能够显示和管理足够的权限。

同样，不同分区之间的重复项也是正常情况。 例如，在为第三方或不同的公司实体发送时，出于不同原因，同一人成为收件人是合乎逻辑的。 但是，在同一分区中查找重复项的情况很少见。

## 产品特定资源

删除地址重复项可保护您的发送信誉，并确保良好的隔离管理。 请参阅以下产品文档部分，了解更多信息：

**Adobe Campaign Classic**

* [重复数据删除活动](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用重复数据删除活动的合并功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=zh-Hans)

**Adobe Campaign Standard**

* [从导入的文件中删除数据重复项](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
