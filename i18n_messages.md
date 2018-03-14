## IMyMessageResolver interface
```

public interface IMyMessageResolver {
	
	public String getMessageForKey(String key);
	
	public String getMessageForKey(MsgKeyEnum key);
	
	public String getMessageForKey(MsgKeyEnum key, Locale locale);
	
	public String getMessageForKey(MsgKeyEnum key, Locale locale, String... params);
	
	public String getMessageForKey(MsgKeyEnum key, String... params);
}

```
## MyMessageResolver Class
```
@Component("messageResolver")
public class MessageResolver implements IMyMessageResolver {
	
	private final Logger mcLogger = LoggerFactory.getLogger(this.getClass());

	@Autowired
	private MessageSource messageSource;

	private String fetchMessage(String key, Locale locale, String[] templateParams) {
		
		try {
			
			String fetchedMsg = messageSource.getMessage(key, templateParams, locale);

			return (fetchedMsg != null ? fetchedMsg : "");
			
		} catch (Exception ex) {
			
			mcLogger.error("Exception occurred while getting message for key: {}", key);
			mcLogger.error("Exception message: {}", ex.getMessage());
		}
		
		return "";
	}

	@Override
	public String getMessageForKey(String key) {
		
		return fetchMessage(key, Locale.US, null);
	}

	@Override
	public String getMessageForKey(MsgKeyEnum key) {
		
		return fetchMessage(key.name(), Locale.US, null);
	}

	@Override
	public String getMessageForKey(MsgKeyEnum key, Locale locale) {
		
		return fetchMessage(key.name(), locale, null);
	}

	@Override
	public String getMessageForKey(MsgKeyEnum key, Locale locale, String... params) {
		
		return fetchMessage(key.name(), locale, params);
	}

	@Override
	public String getMessageForKey(MsgKeyEnum key, String... params) {

		return fetchMessage(key.name(), Locale.US, params);
	}
	
}
```
