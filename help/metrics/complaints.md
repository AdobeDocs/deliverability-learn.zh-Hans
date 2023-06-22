---
title: 投诉
description: 了解当用户指示电子邮件为不需要或意外时登记的投诉。
topics: Deliverability
jira: KT-7048
thumbnail: kt7048.jpg
doc-type: article
activity: understand
team: ACS
exl-id: 0343820d-f5af-4b8a-bcab-dbb47ae7aecb
source-git-commit: 9444f8601f2f349398ee5deb9d5f4d4f7abb44f5
workflow-type: ht
source-wordcount: '0'
ht-degree: 100%

---

# 投诉

当用户指示电子邮件为不需要或意外时，会登记投诉。通常在订阅者点击垃圾邮件按钮时通过订阅者的电子邮件客户端或通过第三方垃圾邮件报告系统记录此订阅者操作。

## ISP 投诉

大多数第 1 层和某些第 2 层 ISP 会向其用户提供垃圾邮件报告方法，因为过去曾恶意使用选择退出和取消订阅过程来验证电子邮件地址。Adobe Campaign 通过 ISP FBL 接收这些投诉。这是在设置过程中为任何提供 FBL 的 ISP 建立的，并允许 Adobe Campaign 自动将受投诉的电子邮件地址添加到隔离表以进行抑制。ISP 投诉数量激增可能表明列表质量差、列表收集方法不太理想或接触策略弱。当内容不相关时，投诉也往往明显激增。

## 第三方投诉

有几个反垃圾邮件组支持在更广泛的级别进行垃圾邮件报告。这些第三方使用的投诉指标用于标记电子邮件内容以识别垃圾电子邮件。此过程也称为指纹识别。这些第三方投诉方法的用户通常对电子邮件更加精通，因此，如果不予回应，他们的影响可能比其他投诉更大。

>[!NOTE]
>
>ISP 会收集投诉并使用它们确定发件人的整体信誉。所有投诉都应加以抑制且尽快不再联系，并应遵照当地法律和法规。

## 产品特定资源

**Adobe Campaign Classic**

* [跟踪指标](https://experienceleague.adobe.com/docs/campaign-classic/using/reporting/reports-on-deliveries/delivery-reports.html?lang=zh-Hans#tracking-indicators)

**Adobe Campaign Standard**

* [投诉报告](https://experienceleague.adobe.com/docs/campaign-standard/using/reporting/list-of-reports/complaints.html?lang=zh-Hans#reporting)
