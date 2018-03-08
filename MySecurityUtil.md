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
