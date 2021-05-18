---
title: IDEA基础配置
date: 2020-06-24 18:32:01
tags:
    - idea
categories: 
  - 雨筝的记录鸭
---

记录以下IDEA的一些初始配置，聊作备忘。

> 记一次重装IDEA

<!-- more -->

## 配置

### 关闭自动更新

路径：File->Settings->Appearance & Behavior->System Settings->Updates

取消 `Automatically check updates for` 勾选 

![update](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/update.png)

### 文件编码设置

路径：File>Settings>Editor>File Encodings

改为`utf-8`

![encodings](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/encodings.png)

###  取消默认打开上次项目

路径：File>Settings>Appearence & Behavior>System Settings

取消 `Reopen last project on startup` 勾选

![start](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/start.png)

### 自动导包和导包优化

路径：File>Settings>Eidtor>General>Auto Import

勾选 `Add unambiguous imports on the fly` 和 `Optimize imports on the fly (for current project)`

![import](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/import.png)


### 类注释模板

路径：File>Settings>Editor>File and Code Templates

![temp](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/temp.png)

```Java
/** 
 * @descriptaion: TODO
 * @author ${USER}
 * @date ${DATE} ${TIME} 
 */
```

### 代码提示快捷键修改

idea的代码提示是`ctrl + 空格`，与win10系统自带快捷键冲突。本人习惯与 `Cyclic Expand Word` 循环往上选择单词） `Alt + /` 替换, 按个人习惯修改,尽量避免冲突

 > 建议尽量保持idea的原生快捷键

路径：File>Settings>Keymap

搜索 `basic` 快速定位

 ![key](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/key.png)

### 提示忽略大小写

 路径：File>Settings>Editor>General>Code Completion

 取消  `match case:` 勾选

  ![match](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/match.png)

### 鼠标滚轮修改字体大小

  路径：File>Settings>Editor>General

  勾选 `Change font size with Ctrl+Mouse Wheel`

  ![fontsize](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/fontsize.png)

### 鼠标悬停提示

  路径：File>Settings>Editor>General>Code Completion

  如图所示:

  ![tip](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/tip.png)

## FAQ

### 连接数据库时区问题

报错 :

```java
Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezone' property manually.
```
解决: 

1. 添加 `serverTimezone=UTC`
   
   ```
    jdbc:mysql://localhost:3306/?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=UTC
   ```

2. 修改 `Advanced`  
   ![tip](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/record/idea-config/db.png)



