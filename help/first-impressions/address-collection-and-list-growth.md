---
title: 解决收集和列表增长问题
description: '了解新电子邮件地址的最佳来源、如何确保高数据质量并符合法律准则。 '
feature: 受众
topics: Deliverability
kt: 7063
thumbnail: kt7063.jpg
doc-type: article
activity: understand
team: TM
translation-type: tm+mt
source-git-commit: 5a15354aa88e34479f2a1ba37cf4dde239c17bd2
workflow-type: tm+mt
source-wordcount: '1598'
ht-degree: 0%

---


# 解决收集和列表增长问题

新电子邮件地址的最佳来源是直接来源，如网站上或实体商店中的注册。 在这些情况下，您可以控制体验以确保体验是积极的，并且订阅者有兴趣从您的品牌获取电子邮件。

有关这些注册方法的一些注意事项：

**由于** 地址输入是口头的或书面的，导致地址拼写错误，因此物理存储列表收集可能会面临挑战。建议在店内注册后尽快发送确认电子邮件。

**网站注册**&#x200B;的最常见形式是“单选登录”。 这是您获取电子邮件地址时应使用的绝对最低标准。 单一选择加入是指特定电子邮件地址的持有人授予发件人发送营销电子邮件的权限，通常是通过Web表单或店内注册提交地址。 虽然使用此方法可以成功运行电子邮件活动，但可能会导致某些问题。

* 未确认的电子邮件地址可能有打字错误或格式错误、不正确或恶意使用。 打字错误和地址格式错误会导致较高的跳出率，这会并会引发ISP或IP信誉损失的阻止。

* 恶意提交已知的垃圾邮件陷阱(有时称为“列表中毒”)，如果陷阱所有者采取行动，会导致投放和声誉出现严重问题。 如果没有确认，就无法确定收件人是否真的希望添加到营销列表。 这同样使得设置收件人的期望变得不可能，并可能导致更多的垃圾邮件投诉 — 如果收集的电子邮件恰好是垃圾邮件陷阱，有时还会黑名单。

有关如何将物理存储和单选加入中的问题降至最低的指导，请访问本指南的[数据质量和卫生](#data-quality-and-hygiene)部分，了解多次选择加入的详细信息和益处。

>[!NOTE]
>
>订阅者经常使用不是他们的扔掉地址、过期地址或地址，以便从网站获得他们想要的内容，同时避免被添加到营销列表。 当发生这种情况时，营销人员的列表会导致大量硬退回、高垃圾邮件投诉率以及不点击、打开或积极参与电子邮件的订阅者。 这可被视为邮箱提供商和ISP的红旗。

## 注册表单

除了要收集有关新订阅者的数据字段外，您还需要收集有关新订阅者的信息，以鼓励与客户建立更相关的联系，此外，您还应在网站上对注册表单执行其他操作。

* 明确期望用户同意接收电子邮件、接收内容以及接收频率。
* 添加选项，使订阅者能够选择他们收到的通信的频率或类型。 这些选项允许您从开始了解订阅者的偏好，以便为新客户提供最佳体验。
* 在注册过程中，通过请求尽可能多的信息来平衡失去用户利益的风险。 他们的生日、地点或兴趣等信息可帮助您发送更多自定义内容。 每个品牌的订阅者都有不同的期望和容忍限度，因此测试是找到适合您的情况的平衡的关键。

>[!NOTE]
>
> 在注册过程中不要使用预选框。 虽然它可能会合法地惹上麻烦，但也是一种负面的客户体验。

## 数据质量和卫生

收集数据只是挑战的一部分。 您必须确保数据准确可用。 您应具有基本格式过滤器。 如果电子邮件地址不包含“@”或“”，则该电子邮件地址无效。 例如。 请务必不允许使用常见别名地址，这些地址也称为角色帐户（如“信息”、“管理员”、“销售”、“支持”等）。 角色帐户可能会带来风险，因为根据其性质，收件人包含一组人，而不是单个订阅者。 预期和容忍度在一个群体中可能有所不同，这可能导致投诉、参与度、取消订阅和普遍混乱。

以下是一些常见问题的解决方案，您可以在电子邮件地址数据中遇到这些问题：

**[!DNL Double opt-in (DOI)]**
[!DNL Double opt-in (DOI)] 被大多数电子邮件专家视为最佳交付能力实践。如果您在欢迎电子邮件中遇到垃圾邮件陷阱或投诉，DOI是确保接收您电子邮件的订户是实际注册您电子邮件项目并希望接收您电子邮件的人的好方法。

DOI包括向订阅者的电子邮件地址发送确认电子邮件，该地址刚注册到您的电子邮件项目，其中包含必须单击才能确认同意的链接。 使用此获取方法，如果用户不确认，发送方将不会向他们发送更多电子邮件。 让新订阅者知道您正在网站上这样做，鼓励他们在继续之前完成注册。 此方法确实会减少注册次数，但是那些注册的用户往往参与度很高，并且会长期停留，并且通常会为发送方带来高得多的ROI。

**隐**
藏字段在注册表单上应用隐藏字段是区分自动机器人注册与实际用户的绝佳方式。由于数据字段不可见（隐藏在HTML代码中），因此机器人将输入人类无法输入的数据。使用此方法，您可以构建规则来禁止任何包含在该隐藏字段中填充的数据的注册。

**[!DNL re-CAPTCHA]**
[!DNL re-CAPTCHA] 是一种验证方法，可用于降低订阅者是bot而非真人的可能性。有各种版本，其中某些版本包含关键字标识或图像。 某些版本比其他版本更有效，而且您在安全性和可交付性问题预防方面所获得的好处远远高于对转化率的任何负面影响。

## 法律准则

请咨询您的律师，以解释有关电子邮件的当地和国家法律。 请记住，电子邮件法律在不同国家/地区之间差异很大，有时一个国家/地区也不同。

* 请务必收集订阅者的位置信息，以便您符合订阅者的国家/地区法律。 如果没有这些细节，您在向订阅者进行营销方面可能会受到限制。
* 任何相关法律均由收件人所在地而非发件人确定。 因此，您需要了解并遵守适用于任何国家/地区的法律，以便拥有订阅者。
* 通常很难完全肯定地了解用户的居住国。 客户提供的数据可能已过时，而像素位置数据可能因VPN或图像仓库而不准确，如Gmail和Yahoo。 如有疑问，最安全的做法是适用最严格的法律和准则。

## 其他不推荐的列表收集方法

收集地址有许多其他方法，每种方法都有其各自的机会、挑战和缺点。 一般不建议使用，因为通常通过提供者可接受的使用策略来限制使用。 我们将看几个常见的例子，以便您了解帮助您限制或避免风险的危险：

**购买或租用列**
表外面有许多类型的电子邮件地址。主电子邮件、工作电子邮件、学校电子邮件、中级电子邮件和不活动的电子邮件等。 通过购买或租用的列表收集和共享的地址类型很少是主要电子邮件帐户，几乎所有参与和购买活动都发生在这些帐户中。

如果你幸运的话，你会得到二级账户，人们在准备购物时会寻找交易和优惠。 这通常会导致参与度较低（如果有）。 如果您不幸运，列表将充满不活动的电子邮件，现在可能是垃圾邮件陷阱。 通常，您会同时收到次要和非活动电子邮件。 一般而言，此类列表的质量弊大于利。 [“Adobe Campaign可接受使用策略”](https://www.adobe.com/legal/terms/aup.html)禁止这种做法。

**列表**
附加这些客户已选择与您的品牌互动，这非常棒。但他们选择通过电子邮件以外的方式（店内、社交媒体等）进行互动。 他们无法接受您未请求的电子邮件，并且可能还担心您是如何获得其电子邮件地址的，因为他们没有提供。 这种方法可能会将参与您品牌的客户或潜在客户转变为不再信任您的品牌并将转而参与您竞争的欺诈者。 [“Adobe Campaign可接受使用策略”](https://www.adobe.com/legal/terms/aup.html)禁止这种做法。

**展会或其他事件收**
集在展位上或通过其他官方、明显带有品牌的方法收集地址非常有用。风险在于，许多像这样的事件会收集所有地址，并通过事件推广者或主机分发这些地址。 这意味着这些电子邮件地址的用户从未请求接收来自您品牌的电子邮件。 这些订阅者可能会投诉您的邮件，并将其标记为垃圾邮件，他们可能没有提供准确的联系信息。

**抽**
奖抽奖可以快速提供大量电子邮件地址。但这些订阅者希望获得奖项，而不是你的电子邮件。 他们甚至可能没有关注谁将与他们接触的名字。 这些订阅者可能会抱怨并将您的邮件标记为垃圾邮件，他们可能永远不会参与或购买邮件。