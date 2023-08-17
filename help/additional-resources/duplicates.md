---
title: 重复
description: 了解如何识别和限制重复项以提高可投放性。
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

* 同一消息被多次发送。 即使Adobe在发送之前默认执行重复数据删除过程，在拆分Target时，也不会阻止具有相同内容的其他操作发送相同的消息。
* 未处理退订请求。 如果收件人在收到消息后取消订阅，则其重复的用户档案仍适用于未来的消息。

除了这种逐步执行选择加入流程外，这种情况还可能导致用户将消息视为垃圾邮件，并在ISP触发ISP阻止列表流程。

在数据库上执行操作时，必须特别谨慎：

* 必须仔细配置导入，尤其是在选择协调键值时。
* 更改的电子邮件地址也可以是重复项的来源。 具体来说，可以将具有不同域的两个地址路由到同一个邮箱，例如，如果一家公司更改了名称并维护了前一个域的特定时间：joe.doe@amce-co.com和joe.doe@acme-rebranded.com。
* 自动导入（无论是列表导入还是从其他数据库导入）都是管理用户档案时要考虑的元素。 当您删除或移动另一个分区中的配置文件时，会发生什么情况？ 可以通过自动导入在初始分区中重新创建它，例如，当下达采购订单时。
* 可以使用视图而不是分区将配置文件存储在不同的文件夹中。 这样，您就可以确保配置式位于同一物理分区中，同时仍然能够显示和管理足够的权限。

同样，在不同的分区之间也存在正常重复的情况。 例如，在发送第三方或不同的公司实体时，出于不同原因，同一个人成为收件人符合逻辑。 但是，在同一分区中查找重复项很少是正常的。

## 产品特定资源

对地址进行重复数据删除可保护您的发送信誉并确保良好的隔离管理。 请在以下产品文档部分了解详情：

**Adobe Campaign Classic**

* [重复数据删除活动](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/targeting-activities/deduplication.html)
* [使用外部重复数据删除活动的合并功能](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/deduplication-merge.html?lang=zh-Hans)

**Adobe Campaign Standard**

* [从导入的文件中删除数据重复项](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/workflow-use-case/data-management/deduplicating-data-imported-file.html)
