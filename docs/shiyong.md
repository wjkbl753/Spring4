业务层接口：

```java
public interface StudentService {
    boolean addStudent(Student student)throws SQLException;
}
```
业务层实现：
```java
@Service
public class StudentServiceImpl implements StudentService{
    @Autowired
    private StudentDAO studentDAO;
    @Override
    public boolean addStudent(Student student) throws SQLException {
        System.out.println("增加学生");
        return true;
    }
}
```

切面：
```java
@Aspect
@Component
public class AopCase {
    @Before("execution(* com.weixin.service.impl.StudentServiceImpl.addStudent(com.weixin.bean.Student))")
    public void before() {
        System.out.println("前置通知");
    }

    @AfterReturning(value = "execution(* com.weixin.service.impl.StudentServiceImpl.addStudent(com.weixin.bean.Student))", returning = "o")
    public void afterReturning(JoinPoint joinPoint, Object o) {
        System.out.println(joinPoint.getSignature().getName());
        System.out.println(o);
        System.out.println("返回通知");
    }

    @AfterThrowing(value = "execution(* com.weixin.service.impl.StudentServiceImpl.addStudent(com.weixin.bean.Student))", throwing = "e")
    public void afterThrowing(JoinPoint joinPoint, Exception e) {
        System.out.println(joinPoint.getSignature().getName());
        System.out.println(e);
        System.out.println("异常通知");
    }

    @After("execution(* com.weixin.service.impl.StudentServiceImpl.addStudent(com.weixin.bean.Student))")
    public void after() {
        System.out.println("后置通知");
    }


/*    @Around("execution(* com.weixin.service.impl.StudentServiceImpl.addStudent(com.weixin.bean.Student))")
    public Object around(ProceedingJoinPoint pjd) {
        System.out.println("前置通知");
        Object result = null;
        try {
            result = pjd.proceed();
            System.out.println("返回通知");
        } catch (Throwable throwable) {
            System.out.println("异常通知");
            throwable.printStackTrace();
        }
        System.out.println("后置通知");
        return result;
    }
}*/
```

测试：
```java
public class StudentTest {
    @Test
    public void test1() throws SQLException {
        ClassPathXmlApplicationContext classPathXmlApplicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
        StudentService studentService = (StudentService)classPathXmlApplicationContext.getBean("studentServiceImpl");
        studentService.addStudent(null);
    }
}
```

