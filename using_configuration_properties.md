## Using Configuration Properties in Service
`application-dev.properties`
```
myprefix.tokenBaseUri= "https://hovermind.com/api/v2/authToken"
myprefix.authHeaderMap.X-xxx-Username= admin
myprefix.authHeaderMap.X-xxx-Password= adminPass

# ... ... ...

```

**Component class to get configuration properties**
```
@ConfigurationProperties(prefix = "myprefix")
@Component("myDevConfig")
public class MyDevConfig {

	private String tokenBaseUri;

	private Map<String, String> authHeaderMap; // should match to myprefix.authHeaderMap
	
	public Map<String, String> getAuthHeaderMap() {
		return authHeaderMap;
	}

	public void setAuthHeaderMap(Map<String, String> authHeaderMap) {
		this.authHeaderMap = authHeaderMap;
	}
	
	public String getTokenBaseUri() {
		return tokenBaseUri;
	}

	public void setTokenBaseUri(String loggedInTelcoName) {
		this.tokenBaseUri = tokenBaseUri;
	}
}
```

**Using in service**
```

@Autowired
MyDevConfig devConfig;

public String getAuthToken(){

	String tokenBaseUri = devConfig.getTokenBaseUri();
	Map<String, String> headerMap = devConfig.getAuthHeaderMap();

	return myRepository.getAuthToken(tokenBaseUri, headerMap);

}

```

## Using Configuration Property in Thymeleaf
`application-dev.properties`
```
myprefix.shouldShowSidebar= 1


# ... ... ...

```

**Component class to get configuration**
```
@ConfigurationProperties(prefix = "myprefix")
@Component("mySidebarConfig")
public class MySidebarConfig {

	private int shouldShowSidebar;
	
	public int getShouldShowSidebar(){
		return shouldShowSidebar;
	}
	
	public void setShouldShowSidebar(int shouldShowSidebar){
		this.shouldShowSidebar = shouldShowSidebar;
	}
}
```

**In thymeleaf master template**
Use `@beanName.property` where `@beanName` refers to a Spring Bean registered at your context.    
(Hint: `@Component("mySidebarConfig")` => `@mySidebarConfig.shouldShowSidebar`)
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head>

<title th:text="${pageTitle}"></title>


</head>

<body>

	<div th:if="${@mySidebarConfig.shouldShowSidebar == 1}">
	
		<!-- side bar here -->
		
	</div>
	
	<div layout:fragment="content" class="container-fluid"></div>

</body>
</html>
```
