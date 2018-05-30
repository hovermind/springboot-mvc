## MongoDB document class
`MyAccount.java`
```
@Document(collection = "account")
public class McAccount {

	@Id
	private String id;
	private String password;
	private String[] roles;

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String[] getRoles() {
		return roles;
	}

	public void setRoles(String[] roles) {
		this.roles = roles;
	}

}

```

## MongoRepository (you can use [MongoTemplate](https://github.com/hovermind/springboot-webmvc/blob/master/mongo_template.md))
`IMyAccountRepository.java`
```
@Repository("myAccountRepository")
public interface IMyAccountRepository extends MongoRepository<MyAccount, String> {
	 Optional<MyAccount> findById(String id);
}
```

Note: [Make sure you created `account` collection](https://github.com/hovermind/springboot-webmvc/blob/master/seed_mongodb.md)
