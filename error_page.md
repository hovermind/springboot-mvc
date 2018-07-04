## Disable white label error page
application.properties
```
# other settings
# ...

server.error.whitelabel.enabled= false
```

## Thymeleaf error pages by convention
If you disable white page, Thymeleaf will pickup pages as following
* `error.html` from views/template folder
* `404.html` from `template/error/404.html` (or `views/error/404.html`)
* `500.html` from `template/error/500.html` (or `views/error/500.html`)

`Create error.html`
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout" layout:decorate="~{layouts/master}">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Error</title>
</head>
<body>

	<div layout:fragment="content">

		<div class="jumbotron">

			<h1>Error Occurred!</h1>
			<p th:text="${'HttpStatus: ' + httpStatus}">
			<p th:text="${message}">

		</div>

	</div>
	
</body>
</html> 
```

## More [Error page total control](https://github.com/hovermind/springboot-webmvc/blob/master/error_page_total_control.md)

## See [Controller Advice to Handle Exception & Error](https://github.com/hovermind/springboot-webmvc/blob/master/controller_advice.md)
