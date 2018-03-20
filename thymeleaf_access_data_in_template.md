## Context variables (attributes of Model/ModelAndView)
Spring Expression Language: `"${key}"`
```
model.addAttribute("key", value); // value == any type

// in template
<p th:text="${key}"></p>
```
## Request parameters
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
Note: `#session` will gives you direct access to the `javax.servlet.http.HttpSession object: ${#session.getAttribute('mySessionAttribute')}`
