## @Component vs @Bean

####  @Component: preferable for component scanning and automatic wiring.

#### When should you use @Bean?
Sometimes automatic configuration is not an option. When? Let's imagine that you want to wire components from 3rd-party libraries (you don't have the source code so you can't annotate its classes with @Component), so automatic configuration is not possible.
The @Bean annotation returns an object that spring should register as bean in application context. The body of the method bears the logic responsible for creating the instance.

## 1 @Autowired by @Component
* Create interface, create class & implements that interfcae
* Annotate class with @Component / @Service / @Repository
* Annotate interface type reference variable with @Autowired
```
public interface IRepo{
  public int getRandomInt();
}

@Repository
public class Repo implements IRepo {

  @Overrride
  public int getRandomInt(){
  
  }
}

@Controller
public class MyController {

  @Autowired
  private IRepo repo;
  
}
```
## 2 @Autowired by @Configuration & @Bean
If you want to customize the class instance registering as bean in application context (for DI) use @Configuration & @Bean annotations    

**Step - 1: Create class (don't mark with @Component / @Repository / @Service)**
```
public class SomeClass {

    private int theNumber;

    public SomeClass(Integer theNumber){
        this.theNumber = theNumber.intValue();
    }

}
```
**Step - 2: Create configuration class and use @Bean**
```
@Configuration
public class SomeConfig{ // SomeClass will be registered in application Context for DI

  @Bean
  public Integer generateNumber(){
      return new Integer(3456);
  }

  @Bean
  public SomeClass someClass(Integer theNumber){
      return new SomeClass(theNumber);
  }
}
```
**Step - 3: Use @Autowired**
```
@Controller
public class MyController {

  @Autowired
  private SomeClass someClass;
  
}
```

