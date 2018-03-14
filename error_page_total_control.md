## Exclude ErrorMvcAutoConfiguration.class
```
@SpringBootApplication(exclude = ErrorMvcAutoConfiguration.class)
public class MyApplication extends SpringBootServletInitializer {

	public static void main(String[] args) {

		ApplicationContext ctx = SpringApplication.run(MyApplication.class, args);

		// 404
		//DispatcherServlet ds = (DispatcherServlet) ctx.getBean("dispatcherServlet");
		//ds.setThrowExceptionIfNoHandlerFound(true);
	}

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(MyApplication.class);
	}
}
```

## 404 when running in STS
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

## 404 when deployed to Tomcat as war
```
@SpringBootApplication(exclude = ErrorMvcAutoConfiguration.class)
public class MyApplication extends SpringBootServletInitializer {

	public static void main(String[] args) {

		ApplicationContext ctx = SpringApplication.run(MyApplication.class, args);

		// 404
		DispatcherServlet ds = (DispatcherServlet) ctx.getBean("dispatcherServlet");
		ds.setThrowExceptionIfNoHandlerFound(true);
	}

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
		return builder.sources(MyApplication.class);
	}
}


@ControllerAdvice
public class MyControllerAdvice {

	private final Logger MyLogger = LoggerFactory.getLogger(this.getClass());

	@ExceptionHandler(NoHandlerFoundException.class)
	public String handle404(NoHandlerFoundException ex) {
		
		MyLogger.debug("NoHandlerFoundException => handle 404");
		
		return "error/404";
	}	
}
```

## 500 - Internal Server Error
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

## Error controller
```
@Controller
public class MyErrorController extends BaseController{

	@GetMapping("error")
	public String error(Model model) {

		model.addAttribute("error", "Test");

		model.addAttribute("status", "444");

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
