## `ServiceGenerator.java`
```
public class ServiceGenerator {

	private static OkHttpClient.Builder httpClient = new OkHttpClient.Builder();

	/**
	 * singleton pattern
	 */
	private ServiceGenerator() {
		// this class is singleton, can not instantiate from other class
	}

	private static Retrofit.Builder getBuilder(String newBaseUri) {

		return new Retrofit.Builder().baseUrl(newBaseUri).addConverterFactory(ScalarsConverterFactory.create()).addConverterFactory(JacksonConverterFactory.create());
	}

	// token authentication
	public static <C> C createService(Class<C> apiClass, String newBaseUri, McAuthToken mcToken) {

		String strAuthToken = getAuthTokenString(mcToken);

		Interceptor interceptor;

		if (McStringUtil.isNotNullEmpty(strAuthToken)) {

			interceptor = new AuthenticationInterceptor(strAuthToken);

			httpClient.addInterceptor(interceptor);

		}

		return getBuilder(newBaseUri).client(httpClient.build()).build().create(apiClass); // return getBuilder(newBaseUri).build().create(apiClass);
	}

	// no authentication
	public static <C> C createService(Class<C> apiClass, String newBaseUri) {
		return createService(apiClass, newBaseUri, null);
	}

	// no authentication & default base uri
	public static <C> C createService(Class<C> apiClass) {
		return createService(apiClass, "http://localhost:8080/api/v1/testserver/");
	}

	public static String getOpenamAuthToken(String openamBaseUri, Map<String, String> headerMap) throws ApiCallException {

		ITokenServiceOpenam ts = createService(ITokenServiceOpenam.class, openamBaseUri);
		Call<OpenamAuthToken> call = ts.getAuthToken(headerMap);

		try {

			Response<OpenamAuthToken> response = call.execute();

			if (response.isSuccessful() && response.body().getTokenId() != null && response.body().getTokenId() != "") {

				return response.body().getTokenId();

			}

			throw new ApiCallException(HttpStatus.valueOf(response.code()), response.message());

		} catch (IOException ex) {

			throw new ApiCallException("IOException while getting auth token from openam");
		}
	}

	private static String getAuthTokenString(AuthToken token) {

		if (token == null) {
			return null;
		}

		if (AuthTokenType.Password.equals(token.getTokenType())) {

			String authToken = null;

			String idOrName = token.getUserIdOrName();
			String password = token.getPassword();

			if (McStringUtil.isNotNullEmpty(idOrName) && McStringUtil.isNotNullEmpty(password)) {
				authToken = Credentials.basic(idOrName, password);
			}

			return authToken;

		} else {

			return token.getAuthToken();
		}
	}
}
```

## `AuthToken.java`
```
public class AuthToken {

	private AuthTokenType tokenType;

	private String authToken;

	private String userIdOrName;

	private String password;

	public AuthTokenType getTokenType() {
		return tokenType;
	}

	public void setTokenType(AuthTokenType tokenType) {
		this.tokenType = tokenType;
	}

	public String getAuthToken() {
		return authToken;
	}

	public void setAuthToken(String authToken) {
		this.authToken = authToken;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public String getUserIdOrName() {
		return userIdOrName;
	}

	public void setUserIdOrName(String userIdOrName) {
		this.userIdOrName = userIdOrName;
	}
}
```
## `AuthTokenType.java`
```
public enum AuthTokenType {
	OAuthToken, Password;
}
```
## `AuthenticationInterceptor.java`
```
public class AuthenticationInterceptor implements Interceptor {

	private String authToken;

	public AuthenticationInterceptor(String authToken) {
		this.authToken = authToken;
	}

	/**
	 * {@inheritDoc}
	 */
	@Override
	public Response intercept(Chain chain) throws IOException {

		Request original = chain.request();

		Request.Builder builder = original.newBuilder().header("User-Agent", "Hovermind").header("Accept", "application/json").header("Content-Type", "application/json").method(original.method(), original.body());

		Request request = builder.build();

		return chain.proceed(request);
	}
}

```
