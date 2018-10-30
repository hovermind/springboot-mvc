## `SecurityConfig.java`
```
@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

	@Autowired
	private AuthSuccessHandler successHandler;

	@Autowired
	private AuthFailureHandler failureHandler;

	@Autowired
	private LogoutHandler logoutHandler;

	@Autowired
	private UserDetailsService userDetailsService;

	@Autowired
	private RoleResolver roleResolver;

	/**
	 * {@inheritDoc}
	 */
	@Override
	protected void configure(HttpSecurity http) throws Exception {

		// csrf: enabled by default when using java config
		// http.csrf().disable();

		// permit all
		http.authorizeRequests().antMatchers(URL_LOGIN_PAGE, URL_LOGOUT_ENDPOINT).permitAll();

		// role base access: dynamically adding access control according to
		// auth-role.roleMap in property file
		String roleKey = "";
		String pageRole = "";
		String pageUrl = "";
		String pageUrlWildcard = "";
		for (Entry<String, String> roleEntry : roleResolver.getRoleMap().entrySet()) {

			roleKey = roleEntry.getKey();
			pageRole = roleEntry.getValue();

			pageUrl = "/" + roleKey;
			pageUrlWildcard = pageUrl + "/**";

			http.authorizeRequests().antMatchers(pageUrl, pageUrlWildcard).hasAuthority(pageRole);
		}
		
		// protect all
		http.authorizeRequests().anyRequest().authenticated();

		// access denied
		http.exceptionHandling().accessDeniedPage(URL_403);

		// Ajax issue when session timed out
		http.exceptionHandling().authenticationEntryPoint(new AjaxAwareAuthEntrypoint(URL_LOGIN_PAGE));

		// form login
		// .defaultSuccessUrl(URL_HOME_PAGE, true);
		http.formLogin().loginPage(URL_LOGIN_PAGE).failureUrl(URL_LOGIN_ERROR).usernameParameter("username").passwordParameter("password").successHandler(successHandler).failureHandler(failureHandler);

		// logout
		http.logout().logoutUrl(URL_LOGOUT_ENDPOINT).invalidateHttpSession(false).clearAuthentication(true).deleteCookies("JSESSIONID").addLogoutHandler(logoutHandler).logoutSuccessUrl(URL_LOGOUT_SUCCESS);

		// Session
		// multiple Login: new session will be created and old user will be redirect to
		// login page
		SessionManagementConfigurer<HttpSecurity> sessionConfig = http.sessionManagement();
		sessionConfig.maximumSessions(1).expiredUrl(URL_INVALID_SESSION);
		sessionConfig.sessionFixation().newSession();
		sessionConfig.invalidSessionUrl(URL_INVALID_SESSION);

		// http.sessionManagement().enableSessionUrlRewriting(true);
		// .deleteCookies("remember-me").rememberMe();
	}

	/**
	 * {@inheritDoc}
	 */
	@Override
	public void configure(AuthenticationManagerBuilder authBuilder) throws Exception {

		authBuilder.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());

	}

	/**
	 * {@inheritDoc}
	 */
	@Override
	public void configure(WebSecurity web) throws Exception {
		web.ignoring().antMatchers("/resources/**", "/static/**", "/bootstrap/**", "/webjars/**", "/jslib/**", "/css/**", "/js/**", "/font/**", "/images/**", "/icons/**", URL_IS_VALID_SESSION);
	}

}
```
