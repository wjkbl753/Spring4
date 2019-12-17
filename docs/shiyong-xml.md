### 切面
```java
public class StudentAop2 {
	public void before(JoinPoint joinPoint) {
		System.out.println("学生增加之前操作--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs()));
	}
	public void afterReturning(JoinPoint joinPoint,Object r) {
		System.out.println("正常返回后执行--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs())+"==返回值"+r);
	}
	public void afterThrowing(JoinPoint joinPoint,Exception e) {
		System.out.println("出现异常时执行--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs())+"异常"+e);
	}
	public void after(JoinPoint joinPoint) {
		System.out.println("后置执行--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs()));
	}
	public Object around(ProceedingJoinPoint pjp) throws Throwable {
		String methodName = pjp.getSignature().getName();
		System.out.println("这是"+methodName+"方法的前置");
		Object result=null;
		try {
			result = pjp.proceed();
			System.out.println("这是"+methodName+"方法的返回通知");
		} catch (Throwable e) {
			System.out.println("这是"+methodName+"方法的异常通知");
			throw e;
		}finally {
			System.out.println("这是"+methodName+"方法的后置通知");
		}
		return result;
	}
}
```
### 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.3.xsd">


<bean id="studentService" class="com.woyuno.service.impl.StudentServiceImpl"/> 
<bean id="myAop" class="com.woyuno.aop.StudentAop2"/>

<aop:config>
	<aop:aspect ref="myAop">
		<aop:pointcut expression="execution(* com.woyuno..service.*.*(..))" id="pointcut"/>
		<!-- <aop:before method="before" pointcut-ref="pointcut"/>
		<aop:after-returning method="afterReturning" pointcut-ref="pointcut" returning="r"/>
		<aop:after-throwing method="afterThrowing" pointcut-ref="pointcut" throwing="e"/>
		<aop:after method="after" pointcut-ref="pointcut"/> -->
		<aop:around method="around" pointcut-ref="pointcut"/>
	</aop:aspect>
</aop:config>
</beans>
```

### 测试
```java
public class StudentTest2 {
	@Test
	public void test1() {
		ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("applicationContext-xml.xml");
		StudentService studentService = (StudentService)classPathXmlApplicationContext.getBean("studentService");
		studentService.add();
		studentService.delete();
	}
}
```