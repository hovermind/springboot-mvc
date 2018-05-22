## Read: [Test Prerequisite](https://github.com/hovermind/springboot-webmvc/blob/master/test_prerequisite.md)

Content from:    
[Testing in Spring Boot](http://www.baeldung.com/spring-boot-testing)    
[Testing Spring Boot Applications](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-testing.html#boot-features-testing-spring-boot-applications)    

## Autowiring just for test
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
**Note:** test class must be annotated with `@RunWith(SpringRunner.class)`    

Interesting thing here is the use of @MockBean. It creates a Mock for the EmployeeRepository which can be used to bypass the call to the actual EmployeeRepository
```
@Before
public void setUp() {
    Employee alex = new Employee("alex");
 
    Mockito.when(employeeRepository.findByName(alex.getName()))
      .thenReturn(alex);
}
```
Since the setup is done, the test case will be simpler:
```
@Test
public void whenValidName_thenEmployeeShouldBeFound() {
    String name = "alex";
    Employee found = employeeService.getEmployeeByName(name);
  
     assertThat(found.getName())
      .isEqualTo(name);
 }
```
## @WebMvcTest - testing controller
```
@RunWith(SpringRunner.class)
@WebMvcTest(EmployeeRestController.class)
public class EmployeeRestControllerTest {
 
    @Autowired
    private MockMvc mvc;
 
    @MockBean
    private EmployeeService service;
 
    // write test cases here
}
```
To test the Controllers, we can use @WebMvcTest. It will auto-configure the Spring MVC infrastructure for our unit tests. In most of the cases, @WebMvcTest will be limited to bootstrap a single controller. It is used along with @MockBean to provide mock implementations for required dependencies.    

@WebMvcTest also auto-configures MockMvc which offers a powerful way of easy testing MVC controllers without starting a full HTTP server.

Having said that, let’s write our test case:
```
@Test
public void givenEmployees_whenGetEmployees_thenReturnJsonArray()
  throws Exception {
     
    Employee alex = new Employee("alex");
 
    List<Employee> allEmployees = Arrays.asList(alex);
 
    given(service.getAllEmployees()).willReturn(allEmployees);
 
    mvc.perform(get("/api/employees")
      .contentType(MediaType.APPLICATION_JSON))
      .andExpect(status().isOk())
      .andExpect(jsonPath("$", hasSize(1)))
      .andExpect(jsonPath("$[0].name", is(alex.getName())));
}
```
The get(…) method call can be replaced by other methods corresponding to HTTP verbs like put(), post(), etc. Please note that we are also setting the content type in the request.    

MockMvc is flexible, and we can create any request using it.    

## @SpringBootTest - integration testing
```
@RunWith(SpringRunner.class)
@SpringBootTest(SpringBootTest.WebEnvironment.MOCK, classes = Application.class)
@AutoConfigureMockMvc
@TestPropertySource(locations = "classpath:application-integrationtest.properties")
public class EmployeeRestControllerIntegrationTest {
 
    @Autowired
    private MockMvc mvc;
 
    @Autowired
    private EmployeeRepository repository;
 
    // write test cases here
}
```
The @SpringBootTest annotation can be used when we need to bootstrap the entire container. The annotation works by creating the ApplicationContext that will be utilized in our tests.

We can use the webEnvironment attribute of @SpringBootTest to configure our runtime environment; we’re using WebEnvironment.MOCK here – so that the container will operate in a mock servlet environment.

We can use the @TestPropertySource annotation to configure locations of properties files specific to our tests. Please note that the property file loaded with @TestPropertySource will override the existing application.properties file.
