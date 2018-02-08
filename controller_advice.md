## Create Controller Advice
MyControllerAdvice.java (see [Custom Exception Classes](https://github.com/hovermind/springboot-webmvc/blob/master/controller_advice.md#custom-exception-classes))
```
@ControllerAdvice
public class MyControllerAdvice {

	private final Logger mcLogger = LoggerFactory.getLogger(this.getClass().getSimpleName());

	/**
	 * Handles controller Exception
	 * 
	 * @param ex
	 *            {@link CommonControllerException}
	 * @param model
	 *            {@link Model}
	 * @return String view name (error.html)
	 */
	@ExceptionHandler(CommonControllerException.class)
	public String handleCommonControllerEx(CommonControllerException ex, Model model) {

		mcLogger.error("Handling CommonControllerException. Error page will be returned");

		model.addAttribute(McConstants.PAGE_TITLE, "Error");

		model.addAttribute(McConstants.HTTP_STATUS, ex.getHttpStatus());
		model.addAttribute(McConstants.MESSAGE, ex.getMessage());

		return "error";
	}

	/**
	 * Handles controller internal server error
	 * 
	 * @param model
	 *            {@link Model}
	 * @return String view name (error.html)
	 */
	@ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
	public String handleInternalServerError(Model model) {
		
		mcLogger.error("Handling INTERNAL_SERVER_ERROR. Error page will be returned");

		model.addAttribute(McConstants.PAGE_TITLE, "Error");

		model.addAttribute(McConstants.HTTP_STATUS, HttpStatus.INTERNAL_SERVER_ERROR);
		model.addAttribute(McConstants.MESSAGE, "Internal server Error occurred");

		return "error";
	}

	/**
	 * Handles Exception related to API call
	 * 
	 * @param ex
	 *            {@link ApiCallException}
	 * @return {@link ResponseEntity} json
	 */
	@ExceptionHandler(ApiCallException.class)
	public ResponseEntity<?> handleApiCallException(ApiCallException ex) {
		
		mcLogger.error("Handling ApiCallException. Error json will be returned");

		return ResponseEntity.status(ex.getHttpStatus()).body(ex.getMessage());
	}
}
```
## Custom Exception Classes
```
public class ApiCallException extends Exception {

	private static final long serialVersionUID = 1L;

	private HttpStatus httpStatus = HttpStatus.INTERNAL_SERVER_ERROR;

	public HttpStatus getHttpStatus() {
		return httpStatus;
	}

	public ApiCallException(String message) {
		super(message);

	}

	public ApiCallException(HttpStatus httpStatus, String message) {
		super(message);

		if (httpStatus != null) {
			this.httpStatus = httpStatus;
		}
	}
}

public class CommonControllerException extends Exception {

	private static final long serialVersionUID = 1L;

	private HttpStatus httpStatus = HttpStatus.INTERNAL_SERVER_ERROR;

	public HttpStatus getHttpStatus() {
		return httpStatus;
	}

	public CommonControllerException(String message) {
		super(message);
	}

	public CommonControllerException(HttpStatus httpStatus, String message) {
		super(message);

		if (httpStatus != null) {
			this.httpStatus = httpStatus;
		}
	}
}
```

## Throwing Custom Exception from RestController
```
@RestController
@RequestMapping("api/v1")
public class TestController {


	@Autowired
	private ITestService testService;

	@PostMapping("test")
	public ResponseEntity<?> testJson(@RequestBody TestRequest tReq) throws Exception {

		try {

			TestResponse tResp = testService.getTestData(tReq.getId());

			if (tResp.getCode() == 200) {

				return ResponseEntity.ok(tResp);

			}

			// api call is ok but status code != 200
			throw new CommonControllerException(e.getMessage());

		} catch (Exception e) {

			// API call related exception
			throw new ApiCallException(e.getMessage());
		}

	}
}
```
