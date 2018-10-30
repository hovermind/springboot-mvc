## Create AjaxAwareAuthEntrypoint
```
public class AjaxAwareAuthEntrypoint extends LoginUrlAuthenticationEntryPoint {

	public AjaxAwareAuthEntrypoint(String loginFormUrl) {
		super(loginFormUrl);
	}

	/**
	 * {@inheritDoc}
	 */
	@Override
	public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {

		if ("XMLHttpRequest".equals(request.getHeader("X-Requested-With"))) {

			response.sendError(403, "Forbidden");

		} else {

			super.commence(request, response, authException);
		}
	}
}
```

## Set AuthenticationEntryPoint in SecurityConfig
```
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    // ...
	
	@Override
	protected void configure(HttpSecurity http) throws Exception {
	
	    //...
		
		// Ajax issue when session timed out
		http.exceptionHandling().authenticationEntryPoint(new AjaxAwareAuthEntrypoint(URL_LOGIN_PAGE));
	}
}
```

**See: [Spring security + Ajax session timeout issue](https://stackoverflow.com/questions/23901950/spring-security-ajax-session-timeout-issue)**
