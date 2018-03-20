## Context variables in Model/ModelAndView
`"${key}"`
```
model.addAttribute("key", value); // value == any type

// in template
<p th:text="${key}"></p>
```
## Request parameters
`"${param.}"`
```
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
