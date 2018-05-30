
In Spring Boot 2.x, one of the main goals was to simplify security configuration and make adding custom security easy. By default, everything is secured, including static resources and Actuator endpoints. If Spring Security is on the classpath, Spring Boot will add @EnableWebSecurity and rely on Spring Security’s content-negoation to decide which authentication mechanism to use. 

Once users decide that they want to add custom security, the default security configuration provided by Spring Boot will back off completely. At this point, users need to explicitly define all the bits they want to secure. This means security configuration is now in one place and avoids any kind of ordering issues with existing WebSecurityConfigurerAdapters.    

Details: [what's new in spring boot 2](http://therealdanvega.com/blog/2018/03/01/what-is-new-spring-boot-2)

## 1. [Seed mongodb](https://github.com/hovermind/springboot-webmvc/blob/master/mongodb_seeding.md)

## 2. Create [MongoRepository](https://github.com/hovermind/springboot-webmvc/blob/master/mongo_repository.md) or [MongoTemplate](https://github.com/hovermind/springboot-webmvc/blob/master/mongo_template.md)

## 3. Create [UserDetails & UserDetailsService](https://github.com/hovermind/springboot-webmvc/blob/master/security_user_details_service.md)

## 4. Create security config class
`MySecurityConfig.java`
```
@Configuration
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

     http
    .authorizeRequests()
        // 1
        .requestMatchers(EndpointRequest.to("status", "info"))
            .permitAll()
        // 2
        .requestMatchers(EndpointRequest.toAnyEndpoint())
            .hasRole("ACTUATOR")
        // 3 
        .requestMatchers(StaticResourceRequest.toCommonLocations())
            .permitAll()
        // 4
        .antMatchers("/**")
            .hasRole("USER")
    .and()
    
    ... // additional configuration
}

```

## 5. Create [authentication handlers](https://github.com/hovermind/springboot-webmvc/blob/master/security_authentication_handler.md) if needed
