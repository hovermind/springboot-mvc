**Note:** for every page there will be a role (all caps) i.e. **Page:** `/admin` => **Role:** `ADMIN` (in db)

## Configuration Property
`application-dev.properties`
```
# ... ... ...

# roleMap
auth-role.roleMap.user= USER
auth-role.roleMap.admin= ADMIN
auth-role.roleMap.abc= ABC

```

## Component Class
`RoleResolver.java`
```
@ConfigurationProperties(prefix = "auth-role")
@Component("roleResolver")
public class RoleResolver {
	
	private Map<String, String> roleMap;

	public Map<String, String> getRoleMap() {
		return roleMap;
	}

	public void setRoleMap(Map<String, String> roleMap) {
		this.roleMap = roleMap;
	}
}
```

## Security Config
`MySecurityConfig.java`
```
@Configuration
public class McSecurityConfig extends WebSecurityConfigurerAdapter {

	@Autowired
	private RoleResolver roleResolver;

	/**
	 * {@inheritDoc}
	 */
	@Override
	protected void configure(HttpSecurity http) throws Exception {
		
		// ... ... ...

		// role base access: dynamically adding access control according to auth-role.roleMap in property file
		String roleKey = "";
		String pageRole = "";
		String pageUrl = "";
		String pageUrlWildcard = "";
		for(Entry<String, String> roleEntry : roleResolver.getRoleMap().entrySet()) {
			
			roleKey = roleEntry.getKey();
			pageRole = roleEntry.getValue();
			
			pageUrl = "/" + roleKey;
			pageUrlWildcard = pageUrl + "/**";
			
			http.authorizeRequests().antMatchers(pageUrl, pageUrlWildcard).hasAuthority(pageRole);
		}
	}
}
```

## Creating Menu Items According to RoleMap in Master Template
Master Template: `master.html`
```
<!--/* ================ generating menu items dynamically depending on roles ======================== */-->

<th:block th:each="role : ${@roleResolver.roleMap}">

	<li th:if="${#authorization.expression('hasAuthority(''' + role.value + ''')')}" th:with="pageUrl=${role.key}">
		<a th:href="@{'/' + ${pageUrl} }" th:id="${'menu_li_' + pageUrl}">[[${#strings.capitalize(pageUrl)}]]</a>
	</li>

</th:block>

<!--/* ================ END of dynamically dynamic menu items generation ======================== */-->
```
**Note: accessing bean in thymeleaf (syntax):** `${@myComponent.property}`

## Loading Fragments According to RoleMap
```
<!--/* ================ generating info blocks dynamically depending on roles ======================== */-->

<th:block th:each="role : ${@roleResolver.roleMap}">

	<div th:replace="${ #authorization.expression('hasAuthority(''' + role.value + ''')') } ? ~{ partials/my_fragments :: __${role.value}__(pageUrl=${role.key}) } : ~{ }"></div>

</th:block>
```

`my_fragments.html`
```
<div th:fragment="USER(pageUrl)" class="service_item_block">

	<div class="flex" th:with="title_lowercase=${pageUrl}">
		<img alt="" th:src="@{'icons/' + ${title_lowercase} + '.png'}" />
		<h3><a th:href="@{'/' + ${pageUrl} }">[[${#strings.capitalize(title_lowercase)}]]</a></h3>
	</div>

	<div class="service_item_content">

	</div>

</div>

<div th:fragment="ADMIN(pageUrl)" class="service_item_block">

	<div class="flex" th:with="title_lowercase=${pageUrl}">
		<img alt="" th:src="@{'icons/' + ${title_lowercase} + '.png'}" />
		<h3><a th:href="@{'/' + ${pageUrl} }">[[${#strings.capitalize(title_lowercase)}]]</a></h3>
	</div>

	<div class="service_item_content">

	</div>

</div>
```
**Note:** put icons in 'static/icons' folder for every fragment (icon name == page url i.e. page: `/abc` => icon: `abc.png`)
