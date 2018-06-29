bean的注入方式主要有以下三种：

- `属性注入 (默认方式，最为常用)`
- `构造注入`
- 工厂方法注入 (不用)

## 属性注入：
```xml
<!-- 配置bean -->
<bean id="student" class="com.weixin.bean.Student">
    <property name="name" value="小明"></property>
    <property name="gender" value="男"></property>
</bean>
```

## 构造注入：
```xml
<!-- 配置bean -->
<bean class="com.weixin.bean.Student">
    <constructor-arg name="name" value="小明"></constructor-arg>
    <constructor-arg name="gender" value="男"></constructor-arg>
</bean>
```

!>如果javabean中属性是int，容器也会自动转换

>学生类中有另一类属性：

学生类有个classroom属性：
```java
public class ClassRoom {
	private int cid;
	private String name;
}
```
`写法1`：
```xml
<!-- 配置教室bean -->
<bean id="classRoom" class="com.weixin.bean.ClassRoom">
    <property name="cid" value="1"></property>
    <property name="name" value="1班"></property>
</bean>

<!-- 配置学生bean -->
<bean class="com.weixin.bean.Student">
    <!-- 使用ref属性建立bean之间的关系 -->
    <property name="classRoom" ref="classRoom"></property>
</bean>
```
`写法2(内部bean)`
```xml
<!-- 配置学生bean -->
<bean class="com.weixin.bean.Student">
    <property name="classRoom">
        <!-- 内部教室bean，但是这个bean不能被其他bean引用 -->
        <bean class="com.weixin.bean.ClassRoom">
            <property name="cid" value="1"></property>
            <property name="name" value="1班"></property>
        </bean>
    </property>
</bean>
```

>学生类中有list,array,set属性：

```java
public class Student {
	private String name;
	private String gender;
    private List<ClassRoom> ClassRooms;
}
```
```xml
<!-- 配置学生bean -->
<bean class="com.weixin.bean.Student">
    <property name="classRooms">
        <list>
            <ref bean="classRoom"/>
            <bean class="com.weixin.bean.ClassRoom">
                <property name="cid" value="2"></property>
                <property name="name" value="2班"></property>
            </bean>
        </list>
    </property>
</bean>
```
!>上面案例list中一个classroom来自外部，一个是内部bean
配置数组也是用list标签，而set集合使用set标签，基本使用一样

>学生类中有map属性：

```java
public class Student {
	private String name;
	private String gender;
    private Map<String,ClassRoom> ;
```
```xml
<property name="map">
    <map>
        <entry key="aa" value-ref="classRoom"></entry>
        <entry key="bb">
            <bean id="classRoom" class="com.weixin.bean.ClassRoom">
                <property name="cid" value="3"></property>
                <property name="name" value="3班"></property>
            </bean>
        </entry>
    </map>
</property>
```

>类中有properties属性

Properties类是Map的一个子接口，多见于第三方类的配置
```java
public class DataSource {
	private Properties properties;
```
```xml
<bean id="dataSource" class="com.weixin.bean.DataSource">
    <property name="properties">
        <props>
            <prop key="url">xxx</prop>
            <prop key="username">user</prop>
            <prop key="password">pass</prop>
            <prop key="driverClass">com.mysql.jdbc.Driver</prop>
        </props>
    </property>
</bean>
```

>使用p命名空间为属性赋值:

```xml
<bean id="student2" class="com.weixin.bean.Student" p:name="小李" p:gender="男"></bean>
```

>使用外部配置数据源：

```xml
<context:property-placeholder location="classpath:db.properties"/>
<bean id="dataSource2" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="jdbcUrl" value="${jdbcUrl}"></property>
    <property name="user" value="${user}"></property>
    <property name="password" value="${password}"></property>
    <property name="driverClass" value="${driverClass}"></property>
</bean>
```
