## IOC(Inversion of Control)

反转资源获取的方向：
传统的资源查找方式是组件向容器发起请求获取资源，而IOC容器是容器主动的将资源推送给它管理的组件。

Spring 容器是 Spring 框架的核心。容器将创建对象，把它们连接在一起，配置它们，并管理他们的整个生命周期从创建到销毁。Spring 容器使用依赖注入（DI）来管理组成一个应用程序的组件。这些对象被称为 Spring Beans

## 配置bean

- xml方式
- 注解方式

>xml方式：

```xml
	<bean id="student" class="com.weixin.bean.Student">
		<property name="name" value="小红"></property>
		<property name="gender" value="女"></property>
	</bean>
```
!> id:是sprignbean的唯一标识，必须唯一，如果不指定。那么id就是这个类的完全限定名,配置后eclipse文件名那里会有标识

!>ioc容器会默认会运用反射的方式，调用实体类中的无参构造方法创建对象并进行维护，然后使用set方法设置属性，所以要求Student类中有无参构造方法

## 获取bean

1. 先获取ioc容器
    - BeanFactory:面向Spring本身
    - ApplicationContext：是BeanFactory的子接口,面向开发者
   
   ApplicationContext的主要实现类：
    - `ClassPathXmlApplicationContext`:从类路径加载配置文件初始化容器
    - FileSystemXmlApplicationContext:从文件系统中加载配置文件初始化容器
```java
ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
```

2. 从ioc容器中获取bean
```java
 applicationContext.getBean(student);
 //此种方式，要求ioc容器中这个类型的bean是唯一的
 applicationContext.getBean(Student.class);
```


