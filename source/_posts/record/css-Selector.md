---
title: css选择器
date: 2020-06-15 11:51:23
description:
tags: 
  - CSS
categories: 
  - 雨筝的记录鸭
---

在 CSS 中，选择器是一种模式，用于选择需要添加样式的元素。

CSS 选择器规定了 CSS 规则会被应用到哪些元素上。

[参考文档](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Selectors)

<!-- more -->

## 基本选择器

### 类型选择器

**概述**

CSS元素选择器(也称为类型选择器)通过node节点名称匹配元素. 因此,在单独使用时,寻找特定类型的元素时,元素选择器都会匹配该文档中所有此类型的元素.

**语法**

按照给定的节点名称，选择所有匹配的元素。
```css
elementname { style properties }
```     

**示例**

下面的css选择所有 `span` 标签：
```css
span {
  color: blue;
}
```

### 类选择器

**概述**

在一个HTML文档中，CSS类选择器会根据元素的类属性中的内容匹配元素。类属性被定义为一个以空格分隔的列表项，在这组类名中，必须有一项与类选择器中的类名完全匹配，此条样式声明才会生效。

**语法**

按照给定的 `class` 属性的值，选择所有匹配的元素。
```css
.classname { style properties }
```

**示例**

下面的css选择所有类型为 `myclass` 的标签：
```css
.myclass {
  background-color: blue;
}
```
> 类型选择器和类选择器组合 

下面的css选择所有 `span` 标签中类型为 `myclass` 的标签：
```css
span.myclass {
  background-color: blue;
}
```

### ID 选择器

**概述**

在一个HTML文档中,CSS ID 选择器会根据该元素的 ID 属性中的内容匹配元素,元素 ID 属性名必须与选择器中的 ID 属性名完全匹配，此条样式声明才会生效。

**语法**

按照 `id` 属性选择一个与之匹配的元素。需要注意的是，一个文档中，每个 ID 属性都应当是唯一的。
```css
#idname{ style properties }
```

> id选择器具有 " `唯一性` " ，但在同一篇文档中 `id` 可以不唯一, `id` 的 `唯一性` 是针对JavaScript而言的。   
>当文档存在多个相同ID时，通过getElementById方法获取到的是在文档中第一个出现该ID的标签(DOM节点对象)，故都推荐ID在文档中最好只出现一次，即“唯一性”。

**示例**

下面的css选择所有id为 `myid` 的标签：
```css
#myid {
  background-color: blue;
}
```

### 通用选择器

**概述**

在CSS中,一个星号( `*` )就是一个通配选择器.它可以匹配任意类型的HTML元素.在配合其他简单选择器的时候,省略掉通配选择器会有同样的效果.比如, `*.current ` 和 `.current` 的效果完全相同.

在CSS3中,星号( `*` )可以和命名空间组合使用
    
**语法**

 1. `*`   - 匹配文档的所有元素
 2. `ns|*`- 会匹配ns命名空间下的所有元素
 3. `*|*` - 会匹配所有命名空间下的所有元素
 4. `|*` - 会匹配所有没有命名空间的元素

> 不推荐使用

### 属性选择器

**概述**

CSS 属性选择器通过已经存在的属性名或属性值匹配元素。

**语法**

按照给定的属性，选择所有匹配的元素。
```css
[attr] [attr=value] [attr~=value] [attr|=value] [attr^=value] [attr$=value][attr*=value]
```

* **[attr]**　：　表示带有以 attr 命名的属性的元素。  
* **[attr=value]** ：　表示带有以 attr 命名的属性，且属性值为 value 的元素。
* **[attr~=value]**　：　表示带有以 attr 命名的属性的元素，并且该属性是一个以空格作为分隔的值列表，其中至少有一个值为 value。    
* **[attr|=value]**　：　表示带有以 attr 命名的属性的元素，属性值为“value”或是以“value-”为前缀（"-"为连字符，Unicode 编码为 U+002D）开头。典型的应用场景是用来匹配语言简写代码（如 zh-CN，zh-TW 可以用 zh 作为 value）。 
* **[attr^=value]**　：　表示带有以 attr 命名的属性，且属性值是以 value 开头的元素。 
* **[attr$=value]**　：　表示带有以 attr 命名的属性，且属性值是以 value 结尾的元素。 
* **[attr\*=value]**　：　表示带有以 attr 命名的属性，且属性值包含有 value 的元素。   
* **[attr operator value i]**　：　在属性选择器的右方括号前添加一个用空格隔开的字母 i（或 I），可以在匹配属性值时忽略大小写（支持 ASCII 字符范围之内的字母）。 
* **[attr operator value s]**　：　在属性选择器的右方括号前添加一个用空格隔开的字母 s（或 S），可以在匹配属性值时区分大小写（支持 ASCII 字符范围之内的字母）。 

> `[attr operator value i]` 与 `[attr operator value s]` 建议不要使用，因为很多浏览器不支持


**示例**

```css
/* 存在title属性的<a> 元素 */
a[title] {
  color: purple;
}

/* 存在href属性并且属性值匹配"https://example.org"的<a> 元素 */
a[href="https://example.org"] {
  color: green;
}

/* 存在class属性并且属性值包含"logo"的<a>元素 */
a[class~="logo"] {
  padding: 2px;
}

/* 存在href属性并且属性值开头是"https"的<a> 元素 */
a[href^="https"] {
  font-style: italic;
}

/* 存在href属性并且属性值结尾是".org"的<a> 元素 */
a[href$=".org"] {
  font-style: italic;
}

/* 存在href属性并且属性值包含"example"的<a> 元素 */
a[href*="example"] {
  font-size: 2em;
}

/* 包含 "insensitive" 的链接,不区分大小写 */
a[href*="insensitive" i] {
  color: cyan;
}

/* 包含 "cAsE" 的链接，区分大小写  建议不用*/ 
a[href*="cAsE" s] { 
  color: pink; 
}

/* 将所有语言为中文的 <div> 元素的文本颜色设为红色
   无论是简体中文（zh-CN）还是繁体中文（zh-TW） 
   建议不用*/
div[lang|="zh"] {
  color: red;
}
```

## 分组选择器

### 选择器列表

**概述**

CSS 选择器列表（ `,` ），常被称为并集选择器或并集组合器，选择所有能被列表中的任意一个选择器选中的节点。

**语法**

`,` 是将不同的选择器组合在一起的方法，它选择所有能被列表中的任意一个选择器选中的节点。

```css
element, element, element { style properties }
```

**示例**

下面的css会同时匹配 `<h1>` 到 `<h6>` 的标签
```css
h1, h2, h3, h4, h5, h6 { 
    font-family: helvetica; 
}
```

## 组合器

### 后代组合器

**概述**

当使用 `␣` 选择符 (这里代表一个空格,更确切的说是一个或多个的空白字符) 连接两个元素时使得该选择器可以只匹配那些由第一个元素作为祖先元素的所有第二个元素(后代元素) . 后代选择器与 子选择器 很相似, 但是后代选择器不需要相匹配元素之间要有严格的父子关系.

**语法**

` `（空格）组合器选择前一个元素的后代节点。
```css
element1 element2{ style properties }
```

**示例**

下面的css会同时匹配 `<div>` 下的所有 `<span>` 标签
```css
div span { 
    background-color: blue; 
}
```
以上的css在如下html中，会匹配所有 `<span>` 标签，包括第二个div里的 `<span>`
```html
<div>
    第二个div
    <span>第一个 span</span>
    <div>
        第二个div
        <span>第二个 span</span>
    </div>
    <span>第三个 span</span>
</div>
```

### 直接子代组合器

**概述**

当使用  `>` 选择符分隔两个元素时,它只会匹配那些作为第一个元素的直接后代(子元素)的第二元素. 与之相比, 当两个元素由 后代选择器 相连时, 它表示匹配存在的所有由第一个元素作为祖先元素(但不一定是父元素)的第二个元素, 无论它在 DOM 中"跳跃" 多少次.

**语法**

`>` 组合器选择前一个元素的直接子代的节点。
```css
element1 > element2{ style properties }
```

**示例**

下面的css会同时匹配 `<div>` 下的所有第一层的 `<span>` 标签
```css
div > span {
  background-color: DodgerBlue;
}
```

以上的css在如下html中，会匹配第一个和第三个 `<span>` 标签，包裹在第二个div里的 `<span>` 不会匹配
```html
<div>
    第二个div
    <span>第一个 span</span>
    <div>
        第二个div
        <span>第二个 span</span>
    </div>
    <span>第三个 span</span>
</div>
```

### 一般兄弟组合器
**概述**

兄弟选择符，位置无须紧邻，只须同层级，`A~B` 选择`A`元素之后所有同层级B元素。

**语法**

`~` 组合器选择兄弟元素，也就是说，后一个节点在前一个节点后面的任意位置，并且共享同一个父节点。
```css
former_element ~ target_element { style properties }
```

**示例**

下面的css会匹配与 `<p>` 同级并在 `<p>` 后面的下的所有 `<span>` 标签
```css
p ~ span {
  color: red;
}
```


以上的css在如下html中，会匹配第二个和第三个 `<span>` 标签，第一个 `<span>` 不会匹配
```html
<span> 第一个 span </span>
<p> 这是个 p </p>
<span> 第二个 span </span>
<span> 第三个 span </span>
```

### 紧邻兄弟组合器

**概述**

相邻兄弟选择器( `+` )介于两个选择器之间，当第二个元素紧跟在第一个元素之后，并且两个元素都是属于同一个父元素的子元素，则第二个元素将被选中。

**语法**

`+` 组合器选择相邻元素，即后一个元素紧跟在前一个之后，并且共享同一个父节点。
```css
former_element + target_element { style properties }
```

**示例**

下面的css会匹配与 `<p>` 同级并在 `<p>` 后面紧邻的 `<span>` 标签
```css
p + span {
  color: red;
}
```


以上的css在如下html中，会匹配第二个 `<span>` 第一和第三个 `<span>` 不会匹配
```html
<span> 第一个 span </span>
<p> 这是个 p </p>
<span> 第二个 span </span>
<span> 第三个 span </span>
```


## 伪选择器

### 伪类

**概述**

CSS 伪类 是添加到选择器的关键字，指定要选择的元素的特殊状态。例如，`:hover` 可被用于在用户将鼠标悬停在按钮上时改变按钮的颜色。

伪类连同伪元素一起，他们允许你不仅仅是根据文档 DOM 树中的内容对元素应用样式，而且还允许你根据诸如像导航历史这样的外部因素来应用样式（例如 `:visited`），同样的，可以根据内容的状态（例如在一些表单元素上的 :checked），或者鼠标的位置（例如 `:hover` 让你知道是否鼠标在一个元素上悬浮）来应用样式。

> **注意**： 与伪类相反，`pseudo-elements`(伪元素) 可被用于为一个元素的 *特定部分* 应用样式


**语法**

`:` 伪选择器支持按照未被包含在文档树中的状态信息来选择元素
```css
selector:pseudo-class {
  property: value;
}
```

**示例**

匹配所有曾被访问过的 <a> 元素
```css
a:visited{
    background-color: blue;
}
```

### 伪元素

**概述**

伪元素是一个附加至选择器末的关键词，允许你对被选择元素的特定部分修改样式。下例中的 `::first-line` 伪元素可改变段落首行文字的样式。

> **注意**：与伪元素比较，`pseudo-classes`(伪类) 能够根据状态改变元素样式。
> 
**语法**

`::` 伪选择器用于表示无法用 HTML 语义表达的实体。
```css
selector::pseudo-element {
  property: value;
}
```

> 一个选择器中只能使用一个伪元素。伪元素必须紧跟在语句中的简单选择器/基础选择器之后。
> 
> **注意**：按照规范，应该使用双冒号（::）而不是单个冒号（:），以便区分伪类和伪元素。但是，由于旧版本的 W3C 规范并未对此进行特别区分，因此目前绝大多数的浏览器都同时支持使用这两种方式来表示伪元素。


**示例**

匹配所有 <p> 元素的第一行。

```css
p::first-line{
    background-color: blue;
}
```
## 选择器优先级

**优先级是如何计算的？**

优先级就是分配给指定的 CSS 声明的一个权重，它由 匹配的选择器中的 每一种选择器类型的 数值 决定。

而当优先级与多个 CSS 声明中任意一个声明的优先级相等的时候，CSS 中最后的那个声明将会被应用到元素上。

当同一个元素有多个声明的时候，优先级才会有意义。因为每一个直接作用于元素的 CSS 规则总是会接管/覆盖（take over）该元素从祖先元素继承而来的规则。

> **注意**: 文档树中元素的接近度（Proximity of elements）对优先级没有影响。

**优先级**

优先级递增：

1. `类型选择器` 和 `伪元素` 
2. `类选择器` 、`属性选择器` 和 `伪类`
3. `ID选择器`
   
通配选择符（`*`）关系选择符（`+`, `>`, `~`, ` `）和 否定伪类（`:not()`）对优先级没有影响。（但是，在 `:not()` 内部声明的选择器会影响优先级）。

给元素添加的内联样式 (例如，`style="font-weight:bold"`) 总会覆盖外部样式表的任何样式 ，因此可看作是具有最高的优先级。

**`!important` 例外规则**

当在一个样式声明中使用一个 `!important` 规则时，此声明将覆盖任何其他声明。

虽然，从技术上讲，`!important` 与优先级无关，但它与最终的结果直接相关。使用 `!important` 是一个坏习惯，应该尽量避免。