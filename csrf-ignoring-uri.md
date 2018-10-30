## Ignoring URI (anonymous user)
**By default spring security would not allow anonymous user**. Use `web.ignoring().antMatchers(...)` to ignore uri for all.
```
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    //...

    @Override
    public void configure(WebSecurity web) throws Exception {
        web.ignoring().antMatchers("/resources/**", "/static/**", "/bootstrap/**", "/webjars/**", "/jslib/**", "/css/**", "/js/**", "/font/**", "/images/**", "/icons/**");
    }
}
```

## Disable CSRF for URI
```
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().ignoringAntMatchers("foo");
    }
    
    //...
}
```
