* [Currupted Jars in .m2 Maven Repository](https://github.com/hovermind/springboot-webmvc/blob/master/spring_boot_troubleshoot.md#currupted-jars-in-m2-maven-repository)
* [Error After Package Name Change](https://github.com/hovermind/springboot-webmvc/blob/master/spring_boot_troubleshoot.md#currupted-jars-in-m2-maven-repository)

## Thymeleaf layout not working
Add thymeleaf-layout-dialect dependency
```
<dependencies>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    
    <dependency>
        <groupId>nz.net.ultraq.thymeleaf</groupId>
        <artifactId>thymeleaf-layout-dialect</artifactId>
    </dependency>
    
</dependencies>
```
## Currupted Jars in .m2 Maven Repository
#### invalid loc header (bad signature)
local m2 repository was corrupted. Manually deleted the local repository and forced maven to download again

#### type org.springframework.core.NestedRuntimeException cannot be resolved.It is indirectly referenced from required .class files
local m2 repository was corrupted. Manually deleted the local repository and forced maven to download again

## Error After Package Name Change
* `application.java` (main class) can not belongs to default package because `@SpringBootApplication` annotation can not be used in class that belongs to default package (actually `@ComponentScan` but `@SpringBootApplication` includes `@ComponentScan`)  
* All other packages should be subpackage of main package (package that contains `application.java`) because `@SpringBootApplication` will scan all subpackages (including package of spring security classes) automatically (no need specifying package names in `@ComponentScan`)
