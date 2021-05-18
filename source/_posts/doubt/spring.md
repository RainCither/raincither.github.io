---
title: 【spring】学习中遇到的一些小问题
date: 2020-08-07 16:29:37
description: 
hide: true
tags:
    - Spring
categories:
    - 雨筝的疑惑哎
---

## 整合 mybatis 时 导入配置文件 连接数据库失败

### 报错

```java
 Access denied for user 'é›¨ç­�'@'localhost' (using password: YES)
```

### 原因

debug  时发现用户名错误
![debug](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/doubt/spring/debug_1.png)
数据库配置文件
![db](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/doubt/spring/db_1.png)
spring配置
![db](https://cdn.jsdelivr.net/gh/RainCither/cdn/top/images/doubt/spring/spring_xml_1.png)


数据库配置文件中使用了 `username` 

spring会默认加载系统使用的环境变量，在spring.xml配置数据库用户名时，实际加载的是当前计算机的名称

### 解决

改个名字就好了

比如加个前缀啥的 `db.username`

或者 在配置文件中加入下面的代码块，就可以**仅调用**配置文件中的参数

```xml
<bean class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer" p:localOverride="true">
    <property name="locations" value="classpath:conn.properties"/>
</bean>
```

