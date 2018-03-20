## Context variables (attributes of Model/ModelAndView)
Spring Expression Language: `"${key}"`
```
model.addAttribute("key", value); // value == any type

// in template
<p th:text="${key}"></p>
```
## Spring beans
`"${@beanName.}"`
```
// spring bean
Component("urlService")
public class UrlService{

}

// in template
<p th:text="${@urlService.getApplicationUrl()}">...</p> 
```
## Session attributes
`"${session.}"` (not `#session`)
```
// in controller
@RequestMapping({"/"})
String index(HttpSession session) {
    session.setAttribute("mySessionAttribute", "someValue");
    return "index";
}

// in template
<p th:text="${session.mySessionAttribute}" th:unless="${session == null}">[...]</p>
```
**Note:** `#session` will gives you direct access to the `javax.servlet.http.HttpSession object: ${#session.getAttribute('mySessionAttribute')}`

## Request parameters (`?q=...`)
`"${param.}"`
```
// in controller
public String redirect() {
    return "redirect:/login?error=true";
}

// in tempate
<p th:if="${param.error}">Error during login, check id/password</p>
```
## HttpServletRequest object
`"${#request.}"`
```
<p th:text="${#request.getParameter('q')}">Param</p>
<p th:text="${#request.getContextPath()}">Context Path</p>
```

## ServletContext attributes
ServletContext attributes are shared between requests and sessions. `"${#servletContext.}"`
```
<p th:text="${#servletContext.getAttribute('myContextAttribute')}">MyContextAttribute</p>
```
**See More:** [Expression Basic Objects](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-a-expression-basic-objects)
