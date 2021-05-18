---
title: 【CSS】学习中遇到的一些小问题
date: 2020-05-09 14:28:39
description: 
tags:
    - CSS
categories:
    - 雨筝的疑惑哎
---

记录学习CSS中的一些小问题与解决方法。

<!-- more -->

## 选择器列表失效

### 原因

在选择器列表中如果有一个选择器不被支持，那么整条规则都会失效。

### 解决

解决这个问题的一个方法是使用 `:is()` 选择器，它会忽视它的参数列表中失效的选择器，但是由于 `:is()`  会影响优先级的计算方式，这么做的代价是，其中的所有选择器都会拥有相同的优先级。

## body内div设置百分比高度失效

### 原因

因为body高度默认值为自适应的，所以设置`body`内`div`高度为百分比是无效的，因为`div`解析上级高度为0，自然`div height 100%`实际高度也为0。

上级元素的高度只是一个缺省值：`height: auto;`。当你要求浏览器根据这样一个缺省值来计算百分比高度时，只能得到`undefined`的结果。也就是一个`null`值，浏览器不会对这个值有任何的反应。

### 解决

如果想让一个元素的百分比css高度height: 100%;起作用，你需要给这个元素的所有父元素的高度设定一个有效值。

```css
/* 设定父元素的高度为100% */
html,body{
    height: 100%;
}             
```

以上虽然实现了`div`的高度为100%，但出现了滚动条，这是因为`body`有默认的`margin`(外边距),所以设置100%高度之后，再加上`margin`(外边距)，就出现下拉滚动条，要想正确显示的话就要把`body`的`margin`(外边距)设置为`0`.

> **提示**：Netscape 和 IE 对 body 标签定义的默认边距（margin）值是 8px。而 Opera 不是这样。相反地，Opera 将内部填充（padding）的默认值定义为 8px，因此如果希望对整个网站的边缘部分进行调整，并将之正确显示于 Opera 中，那么必须对 body 的 padding 进行自定义。

最终实现：
```css
/* 设定父元素的高度为100% */
html,body{
    height: 100%;
}
/* 清除body的外边距 */
body{
    margin: 0;
}
```

<br/>

>或者可以使用绝对定位 `position:fixed;` 来实现  
>可以根据实际情况来做判断使用哪种
>
>   |值|描述|
>   |:----:|:----:|
>   |absolute|生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。<br>元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。|
>   |fixed|生成绝对定位的元素，相对于浏览器窗口进行定位。<br> 元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。|
>   |relative|生成相对定位的元素，相对于其正常位置进行定位。<br> 因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。|
>   |static|默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。|
>   |inherit|规定应该从父元素继承 position 属性的值。|



## 文字超出标签宽度和高度

### 原因

`word-wrap`默认值为`normal`: 只在允许的断字点换行（浏览器保持默认处理，默认值）。


### 解决

修改`word-wrap`的值为`break-word`即可

```css
<div style:"word-wrap:break-word;">内容</div>
```

<br/>

> **word-wrap:** normal|break-word;     
> 
>   |值|描述|
>   |:----:|:----:|
>   |normal|只在允许的断字点换行（浏览器保持默认处理，默认值）。|
>   |break-word|在长单词或 URL 地址内部进行换行。|



## 表格边框之间有间距

### 原因

`td`标签之间有间距

### 解决

设置`td`的间距为0
```css
td{
    border-spacing: 0;
}
```
## JSP文件中导入css失败问题

### 原因

文档树如下：
```bash
webapp
 ├─jsp
 |  ├─css
 |  |  └─style.css
 |  └─index.jsp
 └─WEB-INF
    └─web.xml

```

使用如下语句导入时
```html
<link type="text/css" rel="styleSheet" href="css/style.css" />
```
发现网页并没有加载 `style.css`。


### 解决

导入失败是因为使用的是相对路径，而文件是相对于webapp的，而不是.jsp文件。

**方案一：**

使用相对路径

```html
<link type="text/css" rel="styleSheet" href="jsp/css/style.css" />
```

**方案二：**

使用绝对路径

`${pageContext.request.contextPath}` 相当于当前项目路径，

```html
<link type="text/css" rel="styleSheet" href="${pageContext.request.contextPath}/css/style.css" />
```