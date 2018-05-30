#### [Seed mongodb](https://github.com/hovermind/springboot-webmvc/blob/master/mongodb_seeding.md)

#### Create [MongoRepository](https://github.com/hovermind/springboot-webmvc/blob/master/mongo_repository.md) or [MongoTemplate](https://github.com/hovermind/springboot-webmvc/blob/master/mongo_template.md)

## MyUserDetails.java
```
public class MyUserDetails implements UserDetails {

	private static final long serialVersionUID = 1L;

	private String userName;
	private String password;
	private String salt;

	private List<GrantedAuthority> grantedAuthorities;

	public MyUserDetails(String userName, String password, String salt, List<GrantedAuthority> grantedAuthorities) {
		this.userName = userName;
		this.password = password;
		this.salt = salt;
		this.grantedAuthorities = grantedAuthorities;
	}

	public MyUserDetails(String userName, String password, String salt, String... grantedAuthorities) {
		this.userName = userName;
		this.password = password;
		this.salt = salt;
		this.grantedAuthorities = AuthorityUtils.createAuthorityList(grantedAuthorities);
	}

	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		return grantedAuthorities;
	}

	@Override
	public String getPassword() {
		return password;
	}

	@Override
	public String getUsername() {
		return userName;
	}

	@Override
	public boolean isAccountNonExpired() {
		return true;
	}

	@Override
	public boolean isAccountNonLocked() {
		return true;
	}

	@Override
	public boolean isCredentialsNonExpired() {
		return true;
	}

	@Override
	public boolean isEnabled() {
		return true;
	}

	public String getSalt() {
		return salt;
	}

	public void setSalt(String salt) {
		this.salt = salt;
	}
}
```

## MyUserDetailsService.java
```
@Service("MyUserDetailsService")
public class MyUserDetailsService implements UserDetailsService {

	@Autowired
	private MyAccountRepository accountRepository;

	@Override
	public UserDetails loadUserByUsername(String userName) throws UsernameNotFoundException {

		// Query database & get the user
		MyAccount account = accountRepository.findById(userName);

		// if no user found, throw UserNotFoundException
		if (account == null) {
			throw new UsernameNotFoundException("User does not exist");
		}

		return toUserDetails(account);
	}

	private UserDetails toUserDetails(MyAccount account) {

		MyUserDetails userDetails = new MyUserDetails(account.getId(), account.getPassword(), account.getSalt(), account.getRoles());

		return userDetails;
	}

}

```

## MySecurityConfig.java
```

@Configuration
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

	// custom 403 access denied handler
	@Autowired
	private AccessDeniedHandler accessDeniedHandler;
	
	@Autowired
	private MyAuthenticationSuccessHandler successHandler;
	
	@Autowired
	private MyAuthenticationFailureHandler failureHandler;
	
	@Autowired
	private MyUserDetailsService userDetailsService;
	

	@Override
	protected void configure(HttpSecurity http) throws Exception {
		

        // csrf
        http.csrf().disable();
        
        // permit all
        http.authorizeRequests()
        .antMatchers("/login", "/logout")
        .permitAll()
        .antMatchers("/mgmt/**", "/merchants/**", "/user/**", "/users/**", "/kyc/**", "/openam/**")
        .permitAll();
        
        // role base access
        http.authorizeRequests()
        .antMatchers("/treasury").hasAnyAuthority("BUSSINESS_ADMIN")
        .antMatchers("/subscriber").hasAnyAuthority("CUSTOMER_SUPPORT");
        
        // protect all
        http.authorizeRequests().anyRequest().authenticated();

        // form login
        http.formLogin().loginPage("/login").failureUrl("/login?error=true").usernameParameter("username").passwordParameter("password")
        .successHandler(successHandler).failureHandler(failureHandler);

        // logout
        http.logout()
        .logoutUrl("/logout")
        .logoutSuccessUrl("/login?logout=true")
        .invalidateHttpSession(true)
        .clearAuthentication(true)
        .deleteCookies("JSESSIONID");

        // acces denied
        http.exceptionHandling().accessDeniedPage("/403");//.accessDeniedHandler(accessDeniedHandler);
        
        // seesion
        http.sessionManagement().invalidSessionUrl("/login?invalidSession=true");
        http.sessionManagement().maximumSessions(1).expiredUrl("/login?timeout=true");//.maxSessionsPreventsLogin(true);

        http.sessionManagement().sessionFixation().changeSessionId();
        
        //.deleteCookies("remember-me").rememberMe();
	}

	@Override
	public void configure(AuthenticationManagerBuilder authBuilder) throws Exception {

		//authBuilder.inMemoryAuthentication().withUser("user").password("password").roles("CUSTOMER_SUPPORT").and().withUser("admin").password("password").roles("BUSSINESS_ADMIN");
		//authBuilder.userDetailsService(new MyUserDetailsService()).passwordEncoder(new ShaPasswordEncoder(256));
		
		ReflectionSaltSource saltSource = new ReflectionSaltSource();
		saltSource.setUserPropertyToUse("salt");
		
		DaoAuthenticationProvider authProvider = new DaoAuthenticationProvider();
		authProvider.setSaltSource(saltSource);
		authProvider.setUserDetailsService(userDetailsService);
		authProvider.setPasswordEncoder(new ShaPasswordEncoder(256));
		
		authBuilder.authenticationProvider(authProvider);
		
	}
	
	// @Bean
	// public AuthenticationSuccessHandler successHandler() {
	//
	// SimpleUrlAuthenticationSuccessHandler handler = new SavedRequestAwareAuthenticationSuccessHandler();
	//
	// handler.setUseReferer(true);
	//
	// return handler;
	// }

	
	@Override
	public void configure(WebSecurity web) throws Exception {
		web.ignoring().antMatchers("/resources/**", "/static/**", "/bootstrap/**", "/webjars/**", "/jslib/**", "/css/**", "/js/**", "/font/**", "/images/**", "/icons/**");
	}

}

```

## MyAuthenticationSuccessHandler
```

@Component("myAuthenticationSuccessHandler")
public class MyAuthenticationSuccessHandler implements AuthenticationSuccessHandler {

	private RedirectStrategy redirectStrategy = new DefaultRedirectStrategy();

	@Override
	public void onAuthenticationSuccess(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Authentication authentication) throws IOException, ServletException {

		List<String> roleList = McSecurityUtil.getRolesAsStringList(authentication.getAuthorities());

		String landingPage = McSecurityUtil.getLandingPage(roleList);

		// httpServletResponse.sendRedirect(httpServletRequest.getContextPath() + landingPage);
		redirectStrategy.sendRedirect(httpServletRequest, httpServletResponse, "/" + landingPage);
	}

}

```

## MyAuthenticationFailureHandler
```
@Component("myAuthenticationFailureHandler")
public class MyAuthenticationFailureHandler extends SimpleUrlAuthenticationFailureHandler {

	private RedirectStrategy redirectStrategy = new DefaultRedirectStrategy();

	@Override
	public void onAuthenticationFailure(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
		
		//super.onAuthenticationFailure(request, response, authException);
		
		String userName = request.getParameter("username");
		
		request.getSession(false).setAttribute(MyConstants.LOGIN_LAST_USER_NAME_KEY, userName);

		redirectStrategy.sendRedirect(request, response, "/login?error");

	}

}
```

Helper Methods : [MySecurityUtil.java](https://github.com/hovermind/springboot-webmvc/blob/master/MySecurityUtil.md)

## MyAccessDeniedHandler
```
// handle 403 page
@Component("MyAccessDeniedHandler")
public class MyAccessDeniedHandler implements AccessDeniedHandler {

	private static Logger logger = LoggerFactory.getLogger(MyAccessDeniedHandler.class);

	@Override
	public void handle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, AccessDeniedException e) throws IOException, ServletException {

		Authentication auth = SecurityContextHolder.getContext().getAuthentication();

		if (auth != null) {
			logger.info("User '" + auth.getName() + "' attempted to access the protected URL: " + httpServletRequest.getRequestURI());
		}

		httpServletResponse.sendRedirect(httpServletRequest.getContextPath() + "error/403");

	}
}
```
