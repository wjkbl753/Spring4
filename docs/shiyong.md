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
    public void before(){
        System.out.println("前置通知");
    }
}
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