---
source-git-commit: d105a5b7d81aa14144b9d01f28a5e24c1110ae6c
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 6%

---
# 创建分类规则以支持一键式List-Unsubscribe：

**1。 创建新的分类规则：**

* 在导航树中单击“新建”以创建新分类

![image](/help/assets/CreatingTypologyRules1.png)

**2. 继续配置分类规则：**

* 规则类型：控件
* 阶段：定位开始时
* 渠道：电子邮件
* 级别：您的选择
* 活动


![image](/help/assets/CreatingTypologyRules2.png)


**对分类规则的javascript进行编码：**


>[!NOTE]
>
>下面描述的代码仅作为示例引用。
>此示例详细说明了如何：
>* 配置URL List-Unsubscribe并将添加标头或附加现有mailto：参数并将其替换为： &lt;mailto.。>， https://...
>* 在List-Unsubscribe-Post标头中添加
>发布URL示例使用var headerUnsubUrl = &quot;https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=&lt;%= recipient.cryptedId %>&quot;÷
>* 您可以添加其他参数（如&amp;service = ...）
>


```
// Function to add or replace a header in the provided headers 
function addHeader(headers, header, value)  { 
    
  // Create the new header line 
  var headerLine = header + ": " + value; 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop through each line 
  for (var i=0; i < headerLines.length; i++) { 
      
    // Check if the specified header exists 
    var match = headerLines[i].match(regExp) 
      
    // If it exists 
    if ( match != null ) { 
        
      // Replace the existing header line 
      headerLines[i] = headerLine; 
        
      // Return the modified headers 
      return headerLines.join("\n"); 
    } 
  } 
    
  // If the header does not exist, add the new header line 
  headerLines.push(headerLine); 
    
  // Return the modified headers 
  return headerLines.join("\n"); 
} 
  
// Function to get the value of a specified header from the provided headers 
function getHeader(headers, header) { 
    
  // Create a regular expression to find the specified header 
  var regExp = new RegExp(header + ":(.*)$", "i") 
    
  // Split the headers into individual lines 
  var headerLines = headers.split("\n"); 
    
  // Loop each line 
  for each (line in headerLines) { 
      
    // Check if the specified header exists 
    var match = line.match(regExp); 
      
    // If it exists 
    if ( match != null ) { 
        
      // Return the header value, removing leading whitespace 
      return match[1].replace(/^\s*/, ""); 
    } 
  } 
    
  // If the header does not exist, return an empty string 
  return ""; 
} 
  
  
// Define the unsubscribe URL 
var headerUnsubUrl = "https://campmomentumv7-mkt-prod3.campaign.adobe.com/webApp/unsubNoClick?id=<%= recipient.cryptedId %>"; 
  
// Get the value of the List-Unsubscribe header 
var headerUnsub = getHeader(delivery.mailParameters.headers, "List-Unsubscribe"); 
  
// If the List-Unsubscribe header does not exist 
if ( headerUnsub === "" ) { 
  // Add the List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
// If the List-Unsubscribe header exists and contains 'mailto' 
else if(headerUnsub.search('mailto')){ 
  // Replace the existing List-Unsubscribe header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe", "<"+headerUnsubUrl+">"); 
} 
  
// Get the value of the List-Unsubscribe-Post header 
var headerUnsubPost = getHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post"); 
  
// If the List-Unsubscribe-Post header does not exist 
if ( headerUnsubPost === "" ) { 
  // Add the List-Unsubscribe-Post header 
  delivery.mailParameters.headers = addHeader(delivery.mailParameters.headers, "List-Unsubscribe-Post", "List-Unsubscribe=One-Click"); 
} 
  
// Return true to indicate success 
return true; 
```


![image](/help/assets/CreatingTypologyRules3.png)

**3. 将新规则添加到电子邮件中的分类（默认分类正常）：**

![image](/help/assets/CreatingTypologyRules4.png)

**4。 准备新投放（验证投放属性中的其他SMTP标头是否为空）**

![image](/help/assets/CreatingTypologyRules5.png)

**5。 在投放准备期间检查是否应用了新分类规则。**

![image](/help/assets/CreatingTypologyRules6.png)



**6。 验证List-Unsubscribe是否存在。**

![image](/help/assets/CreatingTypologyRules7.png)
