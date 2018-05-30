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
Helper Methods : [MySecurityUtil](https://github.com/hovermind/springboot-webmvc/blob/master/security_authentication_handler.md#mysecurityutil)

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

## MySecurityUtil
```
public class MySecurityUtil {

	public static String getLandingPage(List<String> roleList) {

		String landingPage = "home";

		if (roleList.contains("ADMIN")) {

			landingPage = "admin";

		} else if (roleList.contains("SUPPORT")) {

			landingPage = "support";
		}

		return landingPage;
	}

	public static List<String> getRolesAsStringList(Collection<? extends GrantedAuthority> authorities) {

		List<String> roleList = new ArrayList<>();

		for (GrantedAuthority authority : authorities) {

			roleList.add(authority.getAuthority());
		}

		return roleList;
	}

}
```
