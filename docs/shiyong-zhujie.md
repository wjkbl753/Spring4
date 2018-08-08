`前置通知-@Before`：该注解标注的方法在业务模块代码执行之前执行，其不能阻止业务模块的执行，除非抛出异常；

`后置通知-@After`：该注解标注的方法在所有的Advice执行完成后执行，无论业务模块是否抛出异常，类似于finally的作用；

`返回通知-@AfterReturning`：该注解标注的方法在业务模块代码执行之后执行；

`异常通知-@AfterThrowing`：该注解标注的方法在业务模块抛出指定异常后执行；


`环绕通知-@Around`：该注解功能最为强大，其所标注的方法用于编写包裹业务模块执行的代码，其可以传入一个ProceedingJoinPoint用于调用业务模块的代码，无论是调用前逻辑还是调用后逻辑，都可以在该方法中编写，甚至其可以根据一定的条件而阻断业务模块的调用；



### 新加入jar包

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>4.3.13.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.1</version>
</dependency>
```
### 配置文件开启包扫描和aop:
```xml
<context:component-scan base-package="com.weixin"/>
<aop:aspectj-autoproxy/>
```
业务层接口：

```java
public interface StudentService {
	int add();
	int delete();
}
```
业务层实现：
```java
@Service
public class StudentServiceImpl implements StudentService{
	@Override
	public int add() {
		System.out.println("==业务层执行add方法====");
		return 1;
	}
	@Override
	public int delete() {
		System.out.println("==业务层执行delete方法====");
		throw new RuntimeException();
	}
}
```

切面：
```java
@Aspect
@Component
public class StudentAop {
	@Before("execution(* com.weixin..service.*.*(..))")
	public void before(JoinPoint joinPoint) {
		System.out.println("学生增加之前操作--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs()));
	}
	@AfterReturning(value="execution(* com.weixin..service.*.*(..))",returning="r")
	public void afterReturning(JoinPoint joinPoint,Object r) {
		System.out.println("正常返回后执行--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs())+"==返回值"+r);
	}
	@AfterThrowing(value="execution(* com.weixin..service.*.*(..))",throwing="e")
	public void afterThrowing(JoinPoint joinPoint,Exception e) {
		System.out.println("出现异常时执行--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs())+"异常"+e);
	}
	@After("execution(* com.weixin..service.*.*(..))")
	public void after(JoinPoint joinPoint) {
		System.out.println("后置执行--方法名:"+joinPoint.getSignature().getName()+"方法参数:"+Arrays.toString(joinPoint.getArgs()));
	}
	/*@Around("execution(* com.weixin..service.*.*(..))")
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
	}*/
}

```
测试：
```java
public class StudentTest {
	@Test
	public void Test1() {
		ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
		 StudentService studentService = (StudentService)classPathXmlApplicationContext.getBean("studentServiceImpl");
		 studentService.add();
		 studentService.delete();
	}
}
```

