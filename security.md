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
