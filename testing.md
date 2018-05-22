## Read: [Test Prerequisite](https://github.com/hovermind/springboot-webmvc/blob/master/test_prerequisite.md)

Annotation for TestClass: `@RunWith(SpringRunner.class)`    

#### Autowiring just for test
During component scanning, we might find components or configurations created only for specific tests accidentally get picked up everywhere. To help prevent that, Spring Boot provides @TestConfiguration annotation that can be used on classes in src/test/java to indicate that they should not be picked up by scanning.    

```
@RunWith(SpringRunner.class)
public class EmployeeServiceImplTest {
 
    @TestConfiguration
    static class EmployeeServiceImplTestContextConfiguration {
  
        @Bean
        public EmployeeService employeeService() {
            return new EmployeeServiceImpl();
        }
    }
 
    @Autowired
    private EmployeeService employeeService;
 
    @MockBean
    private EmployeeRepository employeeRepository;
 
    @Test
    public void exampleTest() {
      //...
    }
}
```
**OR**
```
@RunWith(SpringRunner.class)
@Import(MyTestsConfiguration.class)
public class MyTests {

    @Autowired
    private EmployeeService employeeService;
 
    @MockBean
    private EmployeeRepository employeeRepository;
 
    @Test
    public void exampleTest() {
      //...
    }
}
```
Interesting thing here is the use of @MockBean. It creates a Mock for the EmployeeRepository which can be used to bypass the call to the actual EmployeeRepository
