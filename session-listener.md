## Session listener component
```
@Component("mySessionListener")
public class MySessionListener implements HttpSessionListener {
	
	protected final Logger MyLogger = LoggerFactory.getLogger(this.getClass());

	@Override
	public void sessionCreated(HttpSessionEvent se) {
		
		MyLogger.debug("session started");
	}

	@Override
	public void sessionDestroyed(HttpSessionEvent se) {
		
		MyLogger.debug("session destroyed");
	}

}
```

## Add session listener in Application class
```
@SpringBootApplication(exclude = ErrorMvcAutoConfiguration.class)
public class MyApplication extends SpringBootServletInitializer {

	@Override
	public void onStartup(ServletContext servletContext) throws ServletException {
		super.onStartup(servletContext);

		servletContext.addListener(new MySessionListener());

	}
}
```
