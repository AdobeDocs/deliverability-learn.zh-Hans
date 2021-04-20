---
title: 重复
description: 了解如何识别和限制重复以提高交付能力。
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
source-wordcount: '388'
ht-degree: 3%

---


# 重复{#duplicates}

拥有重复电子邮件地址可能会产生多种后果：

* 同一邮件被多次发送。 即使Adobe在发送之前默认执行外部重复数据删除过程，在拆分目标时，也没有什么可以阻止具有相同内容的不同操作发送相同消息。
* 退订请求未接受。 如果收件人在收到消息后取消订阅，其重复用户档案仍有资格获取将来的消息。

除了选择加入程序的这一侧行，这种情况还可能导致用户将消息视为垃圾邮件，并在ISP触阻止列表发程序。

对数据库执行操作时，必须特别谨慎：

* 必须仔细配置导入，特别是在选择合并关键项时。
* 更改的电子邮件地址也可以是重复源。 特别是，具有不同域的两个地址可以路由到同一邮箱，例如，当公司更改了名称并维护了前一个域一段时间时：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自动导入是管理列表时要考虑的元素，无论是从用户档案还是从其他数据库导入。 删除或移动其他分区中的用户档案时会发生什么情况？ 它可以通过自动导入在初始分区中重新创建，例如，在下订单时。
* 可以使用用户档案而不是分区来实现将视图存储在不同文件夹中。 通过这种方式，您确信用户档案位于同一物理分区中，同时仍允许显示和管理适当的权限。

同样，在不同分区之间的重复是正常的情况。 例如，当为第三方或不同的公司实体发送时，出于不同原因，同一人成为收件人是合乎逻辑的。 但是，在同一分区内查找重复很少是正常的。

## 产品特定资源

消除地址重复可保护您的发送信誉并确保良好的隔离管理。 请阅读以下产品文档部分了解更多信息：

**Adobe Campaign Classic**

* [外部重复数据删除活动](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用外部重复数据删除活动的合并功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html)

**Adobe Campaign Standard**

* [从导入的文件中删除数据重复项](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)