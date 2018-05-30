## MyUserDetails implements UserDetails
```
public class MyUserDetails implements UserDetails {

	private static final long serialVersionUID = 1L;

	private String userName;
	private String password;

	private List<GrantedAuthority> grantedAuthorities;

	public MyUserDetails(String userName, String password, List<GrantedAuthority> grantedAuthorities) {
		this.userName = userName;
		this.password = password;
		this.grantedAuthorities = grantedAuthorities;
	}

	public MyUserDetails(String userName, String password, String... grantedAuthorities) {
		this.userName = userName;
		this.password = password;
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
	
	@Override
	public boolean equals(Object obj) {
		return this.toString().equals(obj.toString());
	}

	@Override
	public int hashCode() {
		return this.userName.hashCode();
	}
}
```

## MyUserDetailsService implements UserDetailsService
prerequisite: [MongoRepository](https://github.com/hovermind/springboot-webmvc/blob/master/mongo_repository.md)
```
@Service("myUserDetailsService")
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class MyUserDetailsService implements UserDetailsService {

	@Autowired
	private IMyAccountRepository accountRepository;

	/**
	 * {@inheritDoc}
	 */
	@Override
	public UserDetails loadUserByUsername(String userName) throws UsernameNotFoundException {

		try {

			// Query database & get the user
			Optional<MyAccount> account = accountRepository.findById(userName);

			// if no user found, throw UserNotFoundException
			if (account.isPresent()) {
				return toUserDetails(account.get());
			} else {
				throw new UsernameNotFoundException("User does not exist.");
			}

		} catch (Exception ex) {

			throw new UsernameNotFoundException(ex.getMessage());
		}

	}

	/**
	 * Converts MyAccount Bean to UserDetails Bean
	 * 
	 * @param account
	 *            Account Bean
	 * @return
	 */
	private UserDetails toUserDetails(MyAccount account) {

		MyUserDetails userDetails = new MyUserDetails(account.getId(), account.getPassword(), account.getRoles());

		return userDetails;
	}

}
```
