#### Spring能够从classpath中自动扫描到bean

注解包括：
- `@Component :除了下面的都拿它标识`
- `@Respository ：持久层`
- `@Service :业务层`
- `@Controller :控制层`

对于扫描到的bean，默认id就是类名首字母小写，当然我们也可以通过value属性指定bean

?>当在类上使用了特定注解后，还需要在配置文件中添加声明：<context:component-scan>

```xml
<!-- base-package表示需要扫描的包及子包 -->
<context:component-scan base-package="com.woyuno.bean"></context:component-scan>
```

子节点： 可以有多个

`<context:include-filter>`  包含特例

`<context:exclude-filter>` 不包含特例
此节点有一个属性type: type的取值annotation(注解) 和assignable(具体接口或类)
```xml
<context:component-scan base-package="com.woyuno.bean">
	<context:exclude-filter type="annotation" expression="org.springframework.stereotype.Component"/>
</context:component-scan>
```
```xml
<context:component-scan base-package="com.woyuno.bean" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Component"/>
</context:component-scan>
```
```xml
<context:component-scan base-package="com.woyuno.bean">
	<context:exclude-filter type="assignable" expression="com.woyuno.dao.StudentDAO"/>
    <!-- 或者 -->
    <!--<context:exclude-filter type="assignable" expression="com.woyuno.dao.impl.StudentDAOImpl"/> -->
</context:component-scan>
```

## 自动装配

现在已经可以使用注解，使ioc容器维护bean，但是bean之间的关联关系我们可以使用 @Autowired
此注解可以写在属性上，构造方法上，setter方法上
spring容器会首先根据变量名=bean的id名去匹配，如果匹配不上再根据类型查找容器中相应的bean进行关联

!>默认容器中必须有匹配bean，如果允许可以没有此类型的bean，那么可以@Autowired(required=false)

!>如果容器中按类型匹配有多个匹配类型，我们一般可以使用@Qualifier指定要注入的bean的id值
```java
	@Autowired
	@Qualifier("studentDAOImpl")
	private StudentDAO studentDAO;
    //或：
    @Autowired
	@Qualifier("studentDAOImpl")
	public void setStudentDAO(StudentDAO studentDAO) {
		this.studentDAO = studentDAO;
	}
    //或：
    @Autowired
	public void setStudentDAO(@Qualifier("studentDAOImpl")StudentDAO studentDAO) {
		this.studentDAO = studentDAO;
	}
```
?> @Autowired匹配规则：
   先查找属性名和bean的id名相同的bean，如果找不到按类型注入 

?> @resource @Inject 和@Autowired用法功能类似，建议使用`@Autowired`

