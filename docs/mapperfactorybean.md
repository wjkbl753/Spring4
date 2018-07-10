?>之前的案例，我们只是省去了SqlSessionFactory和SqlSession的创建，Mapper接口实例仍然需要我们手动调用，所有Mapper也直接让spring管理岂不更好？

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

<context:component-scan base-package="com.weixin"/>
<context:property-placeholder location="classpath:db.properties"/>
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	<property name="driverClass" value="${jdbcDriver}"></property>
	<property name="jdbcUrl" value="${jdbcUrl}"></property>
	<property name="user" value="${user}"></property>
	<property name="password" value="${password}"></property>
</bean>

<!-- sqlsession工厂 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
	<property name="dataSource" ref="dataSource"></property>
	<property name="typeAliasesPackage" value="com.weixin.bean"></property>
	<property name="mapperLocations" value="classpath:mapper/*Mapper.xml"></property>
</bean>

<!-- 直接使用MapperFactoryBean维护Mapper,缺点是得一个一个的配接口 -->
 <bean class="org.mybatis.spring.mapper.MapperFactoryBean">
 	<property name="mapperInterface" value="com.weixin.mapper.StudentMapper"></property>
	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean> 
</beans>
```
[实例](/jar/Spring_Mybatis2.zip ':ignore')