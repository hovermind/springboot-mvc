## Context variables in Model/ModelAndView
```
model.addAttribute("key", value); // value == any type

// in template
<p th:text="${key}"></p>
```
## Request parameters
```
public String redirect() {
    return "redirect:/login?error=true";
}

// in tempate
<p th:if="${param.error}">Error during login, check id/password</p>
```

