## Exclude `ErrorMvcAutoConfiguration.class`
```
@SpringBootApplication(exclude = ErrorMvcAutoConfiguration.class)
public class MyApplication extends SpringBootServletInitializer {

	public static void main(String[] args) {

		ApplicationContext ctx = SpringApplication.run(MyApplication.class, args);
	}

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(MyApplication.class);
	}
}
```

## Deployed to Tomcat as war

#### `Error pages in web.xml`
`src/main/webapp/WEB-INF/web.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	version="3.1">

	<error-page>
		<error-code>404</error-code>
		<location>/404</location>
	</error-page>
	
	<error-page>
		<error-code>500</error-code>
		<location>/500</location>
	</error-page>

</web-app>
```
**`web.xml` sets error page urls and ErrorController methods handle those urls**

#### ErrorController
```
@Controller
public class MyErrorController extends BaseController{

	@GetMapping("error")
	public String error(Model model) {

		model.addAttribute("error", "Test");

		model.addAttribute("status", "000");

		return "error";
	}

	@GetMapping("403")
	public String accessDeniedPage(Model model) {

		return "error/403";
	}

	@GetMapping("404")
	public String notFoundPage(Model model) {

		return "error/404";
	}
	
	@GetMapping("500")
	public String not500(Model model) {

		return "error/500";
	}
}
```
#### 404
* resolved from `web.xml`
* mapped to ErrorController handler method i.e.`@GetMapping("404")`
```
<html xmlns:th="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
	layout:decorate="~{layouts/master}">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<link rel="stylesheet" th:href="@{/css/error_page.css}" />

<title>404</title>
</head>
<body>

	<div layout:fragment="content" align="center">

		<div class="alert alert-danger" style="width: 40%;">

			<div style="margin: 30px;">
				<h2 class="error_page_title">Page not found.</h2>

				<div class="error_page_contents">The requested page was not found.</div>

				<div th:inline="text" class="error_page_last_contents"><a th:href="@{/home}"> Return to Main Page </a></div>
			</div>

		</div>

	</div>

</body>
</html>
```

#### 500
* resolved from `web.xml`
* mapped to ErrorController handler method i.e.`@GetMapping("500")`
```
<html xmlns:th="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
	layout:decorate="~{layouts/master}">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

<link rel="stylesheet" th:href="@{/css/500_page.css}" />

<title>Internal Server Error</title>
</head>
<body>

	<div layout:fragment="content" align="center">

		<div class="alert alert-danger" style="width: 40%;">

			<div style="margin: 30px;">
				<h2 class="error_page_title">Internal Server Error!</h2>

				<div class="error_page_last_contents">Internal server error occurred.</div>
			</div>

		</div>

	</div>

</body>
</html>
```
* or handled by `@ExceptionHandler(Exception.class)` in ControllerAdvice (if not error code not set in `web.xml`)
```
@ControllerAdvice
public class MyControllerAdvice {

	private final Logger MyLogger = LoggerFactory.getLogger(this.getClass());

	@ExceptionHandler(Exception.class)
	public String handleInternalServerError(Model model) {

		return "error/500";
	}
}
```

#### 403
* endpoint set by `HttpSecurity.accessDeniedPage("")` config
* mapped to ErrorController handler method i.e.`@GetMapping("403")`
```
@Configuration
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

	//custom 403 access denied handler
	//@Autowired
	//private AccessDeniedHandler accessDeniedHandler;

	/**
	 * {@inheritDoc}
	 */
	@Override
	protected void configure(HttpSecurity http) throws Exception {

		// csrf
		http.csrf().disable();

		// permit all
		http.authorizeRequests()
			.antMatchers("/login", "/logout").permitAll()

		// role base access
		http.authorizeRequests()
			.antMatchers("/abc", "/abc/**").hasAnyAuthority("MY_ROLE")


		// protect all
		http.authorizeRequests().anyRequest().authenticated();


		// access denied
		http.exceptionHandling()
			.accessDeniedPage("/403");
			//.accessDeniedHandler(accessDeniedHandler);
	}
}
```

## 404 when running in STS/Eclipse
```
@Component
public class MyServletContainerCustomiser implements EmbeddedServletContainerCustomizer {


	@Override
	public void customize(ConfigurableEmbeddedServletContainer esc) {
		
		ErrorPage page404 = new ErrorPage(HttpStatus.NOT_FOUND, "/404");
		
		esc.addErrorPages(page404);

	}
}
```
