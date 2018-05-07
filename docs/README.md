# Spring

Spring 是一个轻量级框架，使用Spring可以简单的实现以前EJB才有的功能，JavaEE的春天

### 内容：
- `依赖注入`：IOC容器
- `面向切面编程`：OP实现
- `数据访问支持`
    - 简化JDBC/ORM
    - 声明式事务
- `WEB集成`

### Spring体系结构:

![](img/1.png)

Spring一共有十几个组件，但是真正的核心组件只有几个

从这个图中我们可以看出Spring框架的核心组件只有三个：Core、Context和Beans。他们构建起了整个Spring的骨骼架构，没有他们就不可能有AOP、Web等上层的特性功能。上面这些是Spring特性功能。。

我列举了比较重要的几个功能：
- `AOP`(主要提供面向切面编程的实现)，
- `Web`(主要提供了Web应用开发的支持及针对Web应用的MVC思想实现) 、
- `ORM`（为我们之前学的Hibernate，以及以后会学到的Mybatis这类持久化框架提供支持）、

其中最最核心的就是AOP和下面Spring核心包，也是我们学习的重点。