## Home page
```
@Configuration
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

	@Override
	protected void configure(HttpSecurity http) throws Exception {

		// other settings

		// form login
		http.formLogin()
			.loginPage("/login")
      
      
			.defaultSuccessUrl("/home", true);
	}
}
```

## Redirect to different pages depending on role
Use AuthenticationSuccessHandler
```
@Configuration
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

	@Autowired
	private McAuthenticationSuccessHandler successHandler;

	@Override
	protected void configure(HttpSecurity http) throws Exception {

		// other settings

		// form login
		http.formLogin()
			.loginPage("/login")
      
      
			.successHandler(successHandler)
	}
}
```

## MyAuthenticationSuccessHandler
```
Component("mcAuthenticationSuccessHandler")
public class MyAuthenticationSuccessHandler implements AuthenticationSuccessHandler {

	protected final Logger mcLogger = LoggerFactory.getLogger(this.getClass());

	@Override
	public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {

		List<String> roleList = MySecurityUtil.getRolesAsStringList(authentication.getAuthorities());
		String landingPage = MySecurityUtil.getLandingPage(roleList); // create util class & use your logic

		String path = request.getContextPath();
		response.sendRedirect(path + "/home");
	}
}
```

