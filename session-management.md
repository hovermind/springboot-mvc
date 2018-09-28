## HttpSessionEventPublisher
`.../WEB-INF/web.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
  http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
  version="3.1">

	
	<listener>
		<listener-class>org.springframework.security.web.session.HttpSessionEventPublisher</listener-class>
	</listener>

</web-app>
```

## WebSecurityConfigurerAdapter
`FooSecurityConfig.java`
```
@Configuration
public class FooSecurityConfig extends WebSecurityConfigurerAdapter {

	private static final String INVALID_URL = "/login?invalidSession=true";

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		// .. ... ...

		// Session
		// Multiple Login: new session will be created and old user will be redirect to login page
		SessionManagementConfigurer<HttpSecurity> sessionConfig = http.sessionManagement();
		sessionConfig.maximumSessions(1).expiredUrl(INVALID_URL);
		sessionConfig.sessionFixation().newSession();
		sessionConfig.invalidSessionUrl(INVALID_URL);
		
		// ... ... ...
	}
}
```

**See:** [Control the Session with Spring Security](https://www.baeldung.com/spring-security-session)
