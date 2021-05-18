---
title: 关于JavaFX的布局类
date: 2020-04-28 22:20:66
description: 记录一下JavaFX中常用的一些布局类的方法
tags: 
  - Java
  - JavaFX
  - 
categories: 
  - 雨筝的记录鸭
---

有关更多：[JavaFX API 文档](https://docs.oracle.com/javase/8/javafx/api/toc.htm)

<!--more-->

## 通用方法

Pane是其它布局控件类的父类，我们可以将Pane看成一个绝对布局控件，这里列举一些布局类的通用方法。

### 设置位置

```java
//设置控件的绝对位置坐标
public final void setLayoutX(double value)
public final void setLayoutY(double value)
```

### 关于高度与宽度

```java
//设置首选高度
protected void setHeight(double value)
//设置首选宽度
protected void setWidth(double value)

//设置最小高度
public final void setMinHeight(double value)
//设置最小宽度
public final void setMinWidth(double value)
//设置最小宽度与高度
public void setMinSize(double minWidth,double minHeight)

//设置最大高度
public final void setMaxHeight(double value)
//设置最大宽度
public final void setMaxWidth(double value)
//设置最大宽度与高度
public void setMaxSize(double maxWidth,double maxHeight)

public final void setPrefHeight(double value)
public final void setPreWidth(double value)
public void setPrefSize(double prefWidth,double prefHeight)
```

### 关于内外边距

```java
//设置此组件内边距
public final void setPadding(Insets value)
//设置子组件的外边距
public static void setMargin(Node child, Insets value)
```

### 获取子组件

```java
public ObservableList<Node> getChildren()
```

### 设置该控件的样式
```java
public final void setStyle(String value)  //设置背景颜色示例：setStyle("-fx-background-color: black;");
```
接受一个类似css样式的字符串表示，与HTML样式属性一样，此变量包含样式属性和值，而不包含样式规则的选择器部分。，且该样式每个控件设置一次即可，多次设置会覆盖上次所设置的样式。   
（关于JavaFX的css的详细参考：[JavaFX CSS Reference Guide](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/doc-files/cssref.html) )


### 设置鼠标监听
```java
public final void setOnMouseClicked(EventHandler<? super MouseEvent> value)
```
定义在此控件上单击（按下并释放）鼠标按钮时要调用的函数。

### 设置键盘监听
```java
//键盘按下时的操作
public final void setOnKeyPressed(EventHandler<? super KeyEvent> value)

//键盘弹起时的操作
public final void setOnKeyReleased(EventHandler<? super KeyEvent> value)
```

定义当此控件或其子控件**具有输入焦点**并按下或弹起键时要调用的函数。
>此控件或其子控件具有输入焦点时才能生效
>>可以对其子组件使用`setOnAction`来获取输入焦点


## AnchorPane

AnchorPane可以将控件锚定到布局面板的某个位置。且跟随窗口的变化而变化，被锚定的组件始终距离窗口的位置不变。
( [api文档](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/layout/AnchorPane.html) )


### 锚定（边距）

```java
//设置子组件到顶部的距离
public static void setTopAnchor(Node child,Double value)
//设置子组件到底部的距离
public static void setBottomAnchor(Node child,Double value)
//设置子组件到左边的距离
public static void setLeftAnchor(Node child,Double value)
//设置子组件到右边的距离
public static void setRightAnchor(Node child,Double value)
```
>此优先级大于`setPadding`

## HBox与VBox

`HBox`可以水平排列控件，不换行。  ( [api文档](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/layout/HBox.html) )
`VBox可`以垂直排列控件，不换列。( [api文档](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/layout/VBox.html) )

### 对齐方式

```java
public final void setAlignment(Pos value)
```

### 组件之间的间距

```java
public final void setSpacing(double value)
```

### 组件大小是否强制一致

```java
//HBox独有
public final void setFillHeight(boolean value)
//VBox独有
public final void setFillWidth(boolean value)
```

## BorderPane

BorderPane将界面分割成上下左右中5部分。当没有组件时会只显示中间，四周不添加组件，默认隐藏。 
( [api文档](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/layout/BorderPane.html) )

### 对齐方式

```java
public final void setAlignment(Pos value)
```

### 设置组件位置

```java 
//设置中间组件
public final void setCenter(Node value)
//设置上方组件
public final void setTop(Node value)
//设置下方组件
public final void setBottom(Node value)
//设置左边组件
public final void setLeft(Node value)
//设置右边组件
public final void setRight(Node value)

```

## FlowPane

FlowPane感觉像HBox和VBox的综合体，FlowPane可以设置一个方向水平或者垂直。默认方向为水平，那么放入FlowPane中的控件会先水平排列，如果第一行满了以后进入下一行继续水平排列。垂直方向类似的，先垂直排列，如果第一列满了以后进入第二列继续垂直排列( [api文档](https://docs.oracle.com/javase/8/javafx/api/javafx/scene/layout/FlowPane.html) )

### 对齐方式

```java
public final void setAlignment(Pos value)
```

### 设置间距

```java
//设置上下间距
public final void setHgap(double value)
//设置左右间距
public final void setVgap(double value)
```

## GridPane

网格布局，需要指定组件添加位置，`getChildren`不可用

### 添加组件
```java

public void add(Node child,int columnIndex,int rowIndex)

public void addRow(int rowIndex,Node... children)

public void addColumn(int columnIndex,Node... children)
```

### 对齐方式

```java
public final void setAlignment(Pos value)
```

### 设置间距

```java
//设置上下间距
public final void setHgap(double value)
//设置左右间距
public final void setVgap(double value)
```