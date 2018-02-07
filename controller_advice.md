

## CustomException
```
public class CustomException extends Exception {

	private static final long serialVersionUID = 1L;

	private HttpStatus httpStatus = HttpStatus.INTERNAL_SERVER_ERROR;

	public HttpStatus getHttpStatus() {
		return httpStatus;
	}

	public CustomException(String message) {
		super(message);
	}

	public CustomException(HttpStatus httpStatus, String message) {
		super(message);
		if (httpStatus != null) {
			this.httpStatus = httpStatus;
		}
	}

}
```

## ControllerAdvice
```
@ControllerAdvice
public class TestControllerAdvice {

	@ExceptionHandler(CustomException.class)
	public ResponseEntity<?> handleCustomException(CustomException ex) {
		
		return ResponseEntity.status(ex.getHttpStatus()).body(ex.getMessage());
	}
	
	/*
	@ExceptionHandler(OtherException.class)
	public ResponseEntity<?> handleOtherException(OtherException ex) {
	
		return ResponseEntity.status(ex.getHttpStatus()).body(ex.getMessage());
	}
	*/
}
```

## Throwing CustomException from RestController
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
			return ResponseEntity.status(tResp.getCode()).body(tResp.getMessage());

		} catch (Exception e) {

			// DB related exception
			throw new CustomException(e.getMessage());
		}

	}
}
```
