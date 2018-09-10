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
Or
```
@Component
public class CustomSuccessHandler extends SimpleUrlAuthenticationSuccessHandler {
 
    private RedirectStrategy redirectStrategy = new DefaultRedirectStrategy();
 
    @Override
    protected void handle(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException {
	
        String targetUrl = determineTargetUrl(authentication);
 
        if (response.isCommitted()) {
            System.out.println("Can't redirect");
            return;
        }
 
        redirectStrategy.sendRedirect(request, response, targetUrl);
    }
 
    protected String determineTargetUrl(Authentication authentication) {
	
        String url = "";
 
        Collection<? extends GrantedAuthority> authorities = authentication.getAuthorities();
 
        List<String> roles = new ArrayList<String>();
 
        for (GrantedAuthority a : authorities) {
            roles.add(a.getAuthority());
        }
 
        if (roles.contains("ROLE_DBA")) {
            url = "/db";
        } else if (roles.contains("ROLE_ADMIN")) {
            url = "/admin";
        } else if (roles.contains("ROLE_USER")) {
            url = "/home";
        } else {
            url = "/accessDenied";
        }
 
        return url;
    }
}
```
