---
title: 【Maven】学习中遇到的一些小问题
date: 2020-05-16 16:22:33
description: 
tags:
    - Maven
categories:
    - 雨筝的疑惑哎
---

## IDEA中启动项目一直Maven报错，找不到符号或 程序包xxx 不存在
`idea2020.1` 莫名BUG，暂时无复现， Maven 导包异常，显示找不到程序包，但代码提示正常，清楚 `IDEA` 配置后恢复。 

de了一天Bug ，最后是`IDEA`的锅。 太难了！

![](https://img-blog.csdnimg.cn/20200503195549476.png)

## Maven报错 不再支持源选项 5。请使用 7 或更高版本。
### 报错

在执行 `mvn package` 时报错

![报错信息](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/doubt/doubt-maven-1.1.png)

### 解决方法

#### 一、在项目的pom.xml文件中指定jdk版本

在 `<project>` 标签下添加 `<properties>` 标签，标签内容如下：

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>13</maven.compiler.source>
    <maven.compiler.target>13</maven.compiler.target>
</properties>
```
>`source` 与 `target` 中的数字为当前使用的JDK版本   
>我当前使用的为`JDK13`，请修改为当前使用版本

#### 二、在全局settings.xml文件中指定jdk版本

`settings.xml` 的位置：     
修改全局：apache-maven\conf\settings.xml    
修改当前用户：当前用户文件夹下的 .m2\settings.xml 

在`settings.xml` 中找到 `<profiles>` 标签，在里面添加 `<profile>` 标签，内容为：

```xml
<profile>  
     <id>jdk-13</id>  
     <activation>  
         <activeByDefault>true</activeByDefault>  
         <jdk>13</jdk>  
     </activation>
     <properties>
         <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
         <maven.compiler.source>13</maven.compiler.source>  
         <maven.compiler.target>13</maven.compiler.target>   
     </properties>   
</profile>  
```
>我当前使用的为`JDK13`，请修改为当前使用版本