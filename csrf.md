## Enable CSRF
`FooSecurityConfig.java`
```
@Configuration
public class MchSecurityConfig extends WebSecurityConfigurerAdapter {

  @Override
  protected void configure(HttpSecurity http) throws Exception {

    // csrf: enabled by default when using java config
    // http.csrf().disable();

  }

  // ... ... ...
}
```
**Note: *Enable CSRF manually when using `xml` config***


## `IllegalStateException` in login page (custom login page)
*can not create a session after the response has already been committed*
#### Reason
a session is required to insert `_csrf` token into login form (`_csrf` token is stored in session, thymeleaf tries to get it from session, since there not session when user comes to login page, a session needs to be created), but part of page has already written to response (to improve performance, output is written to response while parsing - before whole page is parsed => response has already been committed). 
When control comes to `<form>` form tag, thymeleaf tries to create session (inorder to insert `_csrf` token) & then `IllegalStateException` is thrown

#### Solution:
It is the responsibility of Spring Security or the application developer to make sure that when Thymeleaf reaches the point of having to write a CSRF token, a session already exists => manually create session in login controller (`GET` request):
```
@Controller
public class LoginController extends BaseController {

	/**
	 * Handles get request for login page
	 */
	@GetMapping({ "", "/", "login" })
	public String displayRootPageGET(HttpServletRequest request, HttpSession session) {

		if (session == null) {
			if (request.getSession(true) != null) {
				mcLogger.info("Manually creating session to ensure csrf thymeleaf parsing");
			} else {
				mcLogger.info("Tried to create session manually but failed");
			}
		} else {
			mcLogger.info("Session already created to ensure csrf thymeleaf parsing");
		}

		return "login";
	}
}
```

Or [CookieCsrfTokenRepository to prevent eagerly creation of `CsrfToken`](https://github.com/spring-projects/spring-security/issues/3906#issuecomment-222729442)

## Logout Not Working
#### Reason
When CSRF is enabled, only POST method is allowed for logout (`/logout`)

#### Solution:
Wrap logout button with form (`method='POST'`):
```
<form th:action="@{/logout}" method="POST">
	<input type="submit" value="Logout" class="btn btn-danger" />
</form>
```

Of handle logout in java code:
```
@RequestMapping(value="/logout", method = RequestMethod.GET)
public String logoutPage (HttpServletRequest request, HttpServletResponse response) {
    Authentication auth = SecurityContextHolder.getContext().getAuthentication();
    if (auth != null){    
        new SecurityContextLogoutHandler().logout(request, response, auth);
    }
    return "redirect:/login?logout";//You can redirect wherever you want, but generally it's a good practice to show login screen again.
}
```

## Ajax Call not Working
#### Reason
For all post request, CSRF token needed but we can’t submit the CSRF token as a parameter

#### Solution
We can submit the token within the header:
```
// page head
<meta name="_csrf" content="${_csrf.token}"/>
<meta name="_csrf_header" content="${_csrf.headerName}"/>


// Ajax JS
var token = $("meta[name='_csrf']").attr("content");
var header = $("meta[name='_csrf_header']").attr("content");
 
$(document).ajaxSend(function(e, xhr, options) {
    xhr.setRequestHeader(header, token);
});
```
