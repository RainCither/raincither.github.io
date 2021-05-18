---
title: mybatis学习笔记
date: 2020-06-28 14:38:00
tags:
    - Mabatis
categories: 
  - 雨筝的记录鸭
---

## Mybatis简介

* `Mybatis`是一个基于Java的持久层框架。
* 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。
* 可以使用简单的 XML 或注解来配置和映射原生信息

<!-- more -->

## 开始

### 导包

首先要导入Mybatis的所需的jar包

1. 通过Maven导入

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.5</version>
</dependency>
```

2. 手动下载导入

打开链接 [http://github.com/mybatis/mybatis-3/releases](http://github.com/mybatis/mybatis-3/releases) 下载 MyBatis 所需要的包

### 创建xml配置文件

> 更多配置请前往[官方文档](https://mybatis.org/mybatis-3/zh/configuration.html)查看

在`resources`文件夹下新建`mybatis-config.xml`,文件名随意,最好是跟随官方


配置如下:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--  导入配置文件  -->
    <properties resource="db.properties">
        <!--    添加数据，若外部文件中存在则失效，如不存在则添加    -->
        <property name="password" value="admin"/>
    </properties>

    <!-- 设置 -->
    <settings>

        <!-- 设置使用的日志实现  
                常用的有 SLF4J/LOG4J/Log4j2/STDOUT_LOGGING(标准输出日志 不需要配置,可以直接使用) -->
        <setting name="logImpl" value="STDOUT_LOGGING"/>

        <!--  开启二级缓存  -->
        <!-- <setting name="cacheEnabled" value="true"/> -->

        <!--  是否开启驼峰命名自动映射  -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>

        <!--当检测出未知列（或未知属性）时，如何处理，默认情况下没有任何提示，这在测试时很不方便，不容易找到错误。 NONE(默认值)：不做任何处理；WARNING : 警告日志形式的详细信息；FAILING：映射失败，抛出异常和详细信息 -->
        <setting name="autoMappingUnknownColumnBehavior" value="WARNING" />

    </settings>

    <!--  配置实体类别名  -->
    <typeAliases>

        <!-- 方式1：为每个类设置别名 -->
        <typeAlias type="top.raincither.pojo.User" alias="User"/>
        <!-- 方式2：以包中的类名，且将首字母小写作为别名 -->
        <!-- <package name="top.raincither.pojo"/> -->

    </typeAliases>

    <!--  环境配置  default：默认配置  可配置多个环境  -->
    <environments default="development">

        <!-- 环境配置1 -->
        <environment id="development">

            <!--  事务管理器的配置  JDBC/MANAGED -->
            <!--
                JDBC:这个配置直接简单使用了JDBC的提交和回滚设置。它依赖于从数据源得到的连接来管理事务范围。
                MANAGED:这个配置几乎没做什么。一般不会使用。
            -->
            <transactionManager type="JDBC"/>

            <!--  数据源的配置  UNPOOLED/POOLED/JNDI -->    
            <!--     
                UNPOOLED– 这个数据源的实现会每次请求时打开和关闭连接
                POOLED- 数据库连接池
                JNDI – 这个数据源实现是为了能在如 EJB 或应用服务器这类容器中使用
            -->
            <dataSource type="POOLED">

                <!--  取出配置文件中导入的值  -->
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>

        <!-- 环境配置2 -->
        <environment id="development2">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="${driver}" />
                <property name="url" value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />

                <!-- 在任意时间存在的活动连接（也就是正在使用）的数量 -->
                <property name="poolMaximumActiveConnections" value="10" />
                <!-- 任意时间存在的空闲连接数 -->
                <property name="poolMaximumIdleConnections" value="5" />
                <!-- 在被强制返回之前，池中连接被检查的时间 -->
                <property name="poolMaximumCheckoutTime" value="20000" />
                <!-- 这是给连接池一个打印日志状态机会的低层次设置，还有重新尝试获得连接，这些情况下往往需要很长时间（为了避免连接池没有配置时静默失败） -->
                <property name="poolTimeToWait" value="20000" />
                <!-- 发送到数据的侦测查询，用来验证连接是否正常工作，并且准备接受请求 -->
                <property name="poolPingQuery" value="NO PING QUERY SET" />
                <!-- 这是开启或禁用侦测查询。如果开启，你必须用一个合法的SQL语句（最好是很快速的）设置poolPingQuery属性 -->
                <property name="poolPingEnabled" value="false" />
                <!-- 这是用来配置poolPingQuery多少时间被用一次。这可以被设置匹配标准的数据库连接超时时间，来避免不必要的侦测 -->
                <property name="poolPingConnectionsNotUsedFor" value="0" />
            </dataSource>
        </environment>

    </environments>

    <!-- 映射器  -->
    <mappers>
        <!-- 直接映射到相应的mapper文件 -->
        <mapper resource="top/raincither/dao/UserMapper.xml"/>
        <!-- 扫描包路径下所有xxMapper.xml文件 需要接口与xml在同一文件夹下-->
        <package name="top.raincither.dao" />
        <!-- 直接映射到相应的mapper接口 需要接口与xml在同一文件夹下, 一般使用注解用这个-->
        <mapper class="top.raincither.dao.UserMapper" />
    </mappers>
</configuration>
```

所导入的配置文件:

```properties
# JDBC 配置文件
driver = com.mysql.jdbc.Driver
url = jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8
username = root
password = admin

```

### XML 映射器

> 更多配置请前往[官方文档](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html)查看

配置:

```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--  namespace: 命名空间  -->
<!-- 命名空间的作用有两个，一个是利用更长的全限定名来将不同的语句隔离开来，同时实现接口绑定 -->
<!--  top.raincither.dao.UserMapper为UserMapper接口的全限定名  -->
<mapper namespace="top.raincither.dao.UserMapper">


</mapper>

```

SQL 映射文件有很少的几个顶级元素（按照它们应该被定义的顺序）：

* `cache` – 给定命名空间的缓存配置。开启二级缓存
* `cache-ref` – 其他命名空间缓存配置的引用。
* `resultMap` – 是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。
* `sql` – 可被其他语句引用的可重用语句块。
* `insert` – 映射插入语句
* `update` – 映射更新语句
* `delete` – 映射删除语句
* `select` – 映射查询语句

一个简单查询的 select 元素 :

```xml
<select id="getUserById" parameterType="int" resultType="User">
  SELECT * FROM user WHERE id = #{id}
</select>
```

这个语句名(要实现的接口名)为 getUserById ，接受一个 int（或 Integer）类型的参数，并返回一个 User 类型的对象，其中的键是列名，值便是结果行中的对应值。 `#{id}`  是告诉 MyBatis 创建一个预处理语句（PreparedStatement）参数，在 JDBC 中，这样的一个参数在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中。

* `id` 	在命名空间中唯一的标识符，可以被用来引用这条语句。*也就是接口中的方法名*

* `parameterType` 	*将会传入这条语句的参数的类全限定名或别名*。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。*如果传入的是基本类型,则可以不写*

* `resultType` 	*期望从这条语句中返回结果的类全限定名或别名。* 注意，如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身的类型。 resultType 和 resultMap 之间只能同时使用一个。

* `resultMap` 	对外部 resultMap 的命名引用。**结果映射**是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。 resultType 和 resultMap 之间只能同时使用一个。

* `flushCache` 	将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。

* `useCache` 	将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。

* `timeout` 	这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。

**`insert`, `update` 和 `delete` 同理**


## 结果映射（resultMap）

是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。

`ResultMap` 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了。

此为个人简略笔记, 不想写的过于繁琐, 特意简化了非常非常非常多,详见[官方文档](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#Result_Maps)


结果映射归根结底也是使数据库字段与实体类属性对应起来的操作。

(联表)查询时返回数据所对应的实体类中有的属性为数组, 列表 ,对象时或字段名与属性不一致时就要用到结果映射。

**ResultMap 的属性**

* `id` 	当前命名空间中的一个唯一标识，用于标识一个结果映射。
* `type` 要返回的类的完全限定名, 或者一个类型别名（关于内置的类型别名，可以参考上面的表格）。
* `autoMapping` 如果设置这个属性，MyBatis 将会为本结果映射开启或者关闭自动映射。 这个属性会覆盖全局的属性 autoMappingBehavior。默认值：未设置（unset）。 


**ResultMap 的常用元素**

* `result` – 注入到字段或 JavaBean 属性的普通结果
  
* `association` – 一个复杂类型的关联；许多结果将包装成这种类型 也就是对象
    * 嵌套结果映射 – 关联可以是 resultMap 元素，或是对其它结果映射的引用

* `collection` – 一个复杂类型的集合 (列表 集合 数组)
    * 嵌套结果映射 – 集合可以是 resultMap 元素，或是对其它结果映射的引用

* [更多](https://mybatis.org/mybatis-3/zh/sqlmap-xml.html#Result_Maps)
  

### 列子

#### 一个简单的结果映射 (一对一)

这是一个简单的实体类:

```java
package top.raincither.pojo;
public class User {
  private int id;
  private String username;
  private String password;

  public int getId() {
    return id;
  }
  public void setId(int id) {
    this.id = id;
  }
  public String getUsername() {
    return username;
  }N
  public void setUsername(String username) {
    this.username = username;
  }N
  public String getPassword() {
    return password;
  }p
  public void setPassword(String password) {
    this.password = password;
  }
}
```

上面这个类有 3 个属性：id，username 和 Password。如果这些属性与数据库中字段名对应,则这样的一个实体类可以被映射到 `ResultSet` 。

```xml
<select id="getUsers" resultType="top.raincither.pojo.User">
  select id, username, password
  from user
  where id = #{id}
</select>
```

如果与数据库字段名不一致,也可以在语句中设置列别名来完成匹配。而不用显式地配置 `resultMap`

```xml
<select id="getUsers" resultType="top.raincither.pojo.User">
  select
    user_id           as "id",
    user_name         as "username",
    user_password     as "password"
  from user
  where id = #{id}
</select>
```

也可以显式使用外部的 `resultMap`

```xml
<resultMap id="userResultMap" type="User">
  <id property="id" column="user_id" />
  <result property="userName" column="user_name"/>
  <result property="password" column="user_password"/>
</resultMap>
```
然后在引用它的语句中设置 resultMap 属性就行了（注意我们去掉了 resultType 属性）。比如: 

```xml
<select id="getUsers" resultMap="userResultMap">
  select id, username, password
  from user
  where id = #{id}
</select>
```

#### 示例一 (多对一)

如以下查询:

```xml
<select id="getStaff" resultMap="StaffMap">
  select
    b.id  bid,
    b.name bname,
    s.id sid,
    s.name sname
  from boos b , staff s
  where s.tid = t.id
</select>
```

>staff表有 id name tid(外键: boos的id)
>
>boos表有 id name

staff实体类

```java
public class staff {

    private int id;

    private String name;

    private Boos boos;

```

boos实体类

```java
public class boos {

    private int id;

    private String name;

```

可以看到staff类里有三个属性有一个所属的boos对象,但我们查出了四个值,而且并不对应,这时就要用到结果映射。

```xml
<resultMap id="StaffMap" type="staff">
    <!--映射基本类型属性 -->
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <!-- association: 映射对象  JavaType:要映射到的实体类完全限定名,或别名-->
    <association property="boos" javaType="Boos">
        <result property="id" column="bid"/>
        <result property="name" column="bname"/>
    </association>
</resultMap>

<select id="getStaff" resultMap="StaffMap">
    select
        b.id  bid,
        b.name bname,
        s.id sid,
        s.name sname
    from boos b , staff s
    where s.tid = t.id
</select>

```

#### 示例二 (一对多)

staff实体类

```java
public class staff {

    private int id;

    private String name;

```

boos实体类

```java
public class boos {

    private int id;

    private String name;

    private List<staff> staffs;

```

如上 boos 实体类内有属性为列表

```xml
<resultMap id="BoosMap" type="Boos">
    <result property="id" column="bid"/>
    <result property="name" column="bname"/>
    <!-- collection: 映射列表  JavaType:要映射到的实体类完全限定名,或别名 ofType: 泛型-->
    <collection property="staffs"  javaType="ArrayList" ofType="Staff">
        <result property="id" column="sid"/>
        <result property="name" column="sname"/>
    </collection>
</resultMap>

<select id="getBoosById" resultMap="BoosMap">
    select s.id sid, s.name sname, b.id bid, b.name tname
    from staff s,
         boos b
    where b.id = s.tid
        and b.id = #{id};
</select>
```

## 缓存

### 一级缓存

* 默认开启一级缓存

* 在同一个 `SqlSession` 中, `Mybatis` 会把执行的方法和参数通过算法生成缓存的键值， 将键值和结果存放在一个 `Map` 中， 如果后续的键值一样， 则直接从 `Map` 中获取数据
  
* 不同的 `SqlSession` 之间的缓存是不互通的, 也就是说 缓存只在当前 `SqlSession` 有效

* 同一个 `SqlSession`， 可以通过配置 `flushCache = 'true` 使得在查询前清空缓存

  
* `UPDATE`, `INSERT`, `DELETE` 语句默认会清空缓存

### 二级缓存

二级缓存需要在 `mybatis-config.xml` 中开启总开关 默认开启

```xml
    <!--  开启二级缓存  -->
  <setting name="cacheEnabled" value="true"/>
```

还要在 `*Mapper.xml` 开启 默认是关闭的

```xml
<!-- 开启二级缓存 -->
<!--  <cache/>  -->

<!-- 所有配置项 -->
<!-- 
    eviction: 清除策略
    
    LRU – 最近最少使用：移除最长时间不被使用的对象。
    FIFO – 先进先出：按对象进入缓存的顺序来移除它们。
    SOFT – 软引用：基于垃圾回收器状态和软引用规则移除对象。
    WEAK – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象。

    flushInterval: 刷新间隔  以毫秒为单位的合理时间量 默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新。 

    size: 引用数目 为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源。
            默认值是 1024。 

    readOnly: 只读 为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。
             因此这些对象不能被修改。这就提供了可观的性能提升。
             而可读写的缓存会（通过序列化）返回缓存对象的拷贝。 速度上会慢一些，但是更安全，
             因此默认值是 false。 
-->
<cache
  eviction="FIFO"
  flushInterval="60000"
  size="512"
  readOnly="true"/>
```

* 需要手动设置启动二级缓存

* 二级缓存的作用域是同一个namespace下的mapper映射文件 多个SqlSession共享  

    * 由于二级缓存中的数据是基于namespace的，即不同namespace中的数据互不干扰。在多个namespace中若均存在对同一个表的操作，那么这多个namespace中的数据可能就会出现不一致现象。

* 增删改操作，无论是否进行提交sqlSession.commit()，均会清空一级、二级缓存，使查询再次从DB中select。 
  
  * 二级缓存的清空，实质上是对所查找key对应的value置为null
  
* 只在单表上使用二级缓存 
  
    * 如果一个表与其它表有关联关系，那么久非常有可能存在多个namespace对同一数据的操作。而不同namespace中的数据互补干扰，所以就有可能出现多个namespace中的数据不一致现象
* 在查询多于修改时使用二级缓存 
  
    * 在查询操作远远多于增删改操作的情况下可以使用二级缓存。因为任何增删改操作都将刷新二级缓存，对二级缓存的频繁刷新将降低系统性能。



## 动态Sql



对于比较复杂的业务，我们需要写复杂的 SQL 语句，往往需要拼接，而拼接 SQL ，例如拼接时要确保不能忘记添加必要的空格，还要注意去掉列表最后一个列名的逗号。稍微不注意，由于引号，空格等缺失可能都会导致错误。

利用动态 SQL，可以彻底摆脱这种痛苦。

> 其实动态 sql 语句的编写往往就是一个拼接的问题，为了保证拼接准确，我们最好首先要写原生的 sql 语句出来，然后在通过 mybatis 动态sql 对照着改，防止出错。

动态sql元素:

*   if
*   choose (when, otherwise)
*   trim (where, set)
*   foreach


> 示例用数据库字段 :    
> 　　　　user:             
>　　　　　　id 　name　　age　　city


### if

`if` 如果条件为正则追加

根据 name 和 age 来查询数据。如果 name 为空，那么将只根据 age 来查询；反之如果 age 为空只根据 name 来查询 

```xml
<select id="getUserByNameAndAge" resultType="User" parameterType="User">
    select * from user where 
        <if test="name != null">
           username=#{name}
        </if>
         
        <if test="age != null">
           and age=#{age}
        </if>
</select>
```

如果 age 为空那么查询语句为 `select * from user where name=#{name}`  这没问题 但当name为空时 查询语句为 `select * from user where and age=#{age}`，这是错误的 

这是不可行的,解决的方法就是 `where`

### where

`where` 如果它包含的标签中有返回值的话，它就插入一个‘where’ , 此外，如果标签返回的内容是以AND 或OR 开头的，则它会剔除掉。

> 在 sql 片段中最好不要包括 where 

```xml
<select id="getUserByNameAndAge" resultType="User" parameterType="User">
    select * from user
    <where>
        <if test="name != null">
           and username=#{name}
        </if>
         
        <if test="age != null">
           and age=#{age}
        </if>
    </where>
</select>
```

这样当都为空时,就会查出所有数据

### set

`set` 同理 如果它包含的标签中有返回值的话，它就插入一个‘set’ , 此外，如果标签返回的内容是以AND 或OR 开头的，则它会剔除掉。

```xml
<update id="updateUserById" parameterType="User">
    select * from user
    <set>
        <if test="name != null">
           username=#{name},
        </if>
         
        <if test="age != null">
           age=#{age},
        </if>
    </set>
    where id = #{id}
</update>
```

### choose(when,otherwise)

`choose(when,otherwise)` 有时候，我们不想用到所有的查询条件，只想选择其中的一个，查询条件有一个满足即可，使用 choose 标签可以解决此类问题，类似于 Java 的 switch 语句

```xml
<select id="getUserByChoose" resultType="User" parameterType="User">
      select * from user
      <where>
          <choose>
              <when test="id != null">
                  id=#{id}
              </when>
              <when test="name != null">
                  and name=#{name}
              </when>
              <otherwise>
                  and age=#{age}
              </otherwise>
          </choose>
      </where>
  </select>
```

也就是说，这里我们有三个条件，id,name,age，只能选择一个作为查询条件

当前两个都不为null的时候， 那么选择二选一（前者优先）,如果都为null, 那么就选择 otherwise中的
如果前两个只有一个不为null, 那么就选择不为null的那个。


### foreach 

`foreach` java中有for, 可通过for循环， 同样， mybatis中有foreach, 可通过它实现循环


将一个 List 实例或者数组作为参数对象传给 MyBatis,MyBatis 会自动将它包装在一个 Map 中

```xml
<select id="get" resultType="User" parameterType="map">
    select *
    from user
    where id in
    <foreach item="item" index="index" collection="list"
        open="(" separator="," close=")">
        #{item}
    </foreach>
</select>
```
 你可以将任何可迭代对象（如 List、Set 等）、Map 对象或者数组对象作为集合参数传递给 foreach。当使用可迭代对象或者数组时，index 是当前迭代的序号，item 的值是本次迭代获取到的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。

### 更多

更多详情,请见[官方文档](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)

## 注解开发

注解只适用于 较短sql , 而且不易维护 ,建议不用

```java

//首先在mybatis-config.xml中 注册接口类

@select("select * from user")
List<User> getUserList();

```

