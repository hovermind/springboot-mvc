## MyAccount.java for mongodb (Collection)
```
@Document(collection = "account")
public class MyAccount {

	@Id
	private String id;
	private String password;
	private String salt;
	private String[] roles;
	
	// add getter setter
}
```
## IMyAccountRepository.java to get data from mongodb
```

@Repository("myAccountRepository")
public interface IMyAccountRepository extends MongoRepository<MyAccount, String> {
	 MyAccount findById(String id);
}

```

## MyUserDetails.java for UserDetailsService
```
public class MyUserDetails implements UserDetails {

	private static final long serialVersionUID = 1L;

	private String userName;
	private String password;
	private String salt;

	private List<GrantedAuthority> grantedAuthorities;

	public MyUserDetails(String userName, String password, String salt, List<GrantedAuthority> grantedAuthorities) {
		this.userName = userName;
		this.password = password;
		this.salt = salt;
		this.grantedAuthorities = grantedAuthorities;
	}

	public MyUserDetails(String userName, String password, String salt, String... grantedAuthorities) {
		this.userName = userName;
		this.password = password;
		this.salt = salt;
		this.grantedAuthorities = AuthorityUtils.createAuthorityList(grantedAuthorities);
	}

	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		return grantedAuthorities;
	}

	@Override
	public String getPassword() {
		return password;
	}

	@Override
	public String getUsername() {
		return userName;
	}

	@Override
	public boolean isAccountNonExpired() {
		return true;
	}

	@Override
	public boolean isAccountNonLocked() {
		return true;
	}

	@Override
	public boolean isCredentialsNonExpired() {
		return true;
	}

	@Override
	public boolean isEnabled() {
		return true;
	}

	public String getSalt() {
		return salt;
	}

	public void setSalt(String salt) {
		this.salt = salt;
	}
}
```

## MyUserDetailsService.java
```
@Service("MyUserDetailsService")
public class MyUserDetailsService implements UserDetailsService {

	@Autowired
	private MyAccountRepository accountRepository;

	@Override
	public UserDetails loadUserByUsername(String userName) throws UsernameNotFoundException {

		// Query database & get the user
		MyAccount account = accountRepository.findById(userName);

		// if no user found, throw UserNotFoundException
		if (account == null) {
			throw new UsernameNotFoundException("User does not exist");
		}

		return toUserDetails(account);
	}

	private UserDetails toUserDetails(MyAccount account) {

		MyUserDetails userDetails = new MyUserDetails(account.getId(), account.getPassword(), account.getSalt(), account.getRoles());

		return userDetails;
	}

}

```
