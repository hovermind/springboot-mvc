## Getting context path (end point after domain)
```
// template
<meta name="ctx" th:content="${#httpServletRequest.getContextPath()}" />

// JS util
function getContextPathWithSlash(){
	
	let ctxPath = $('meta[name="ctxPath"]').attr("content");
	
	if(!ctxPath){
		return '/';
	}
	
	return ctxPath + '/';
}
```
## Adding context path to all Ajax call
```
// Prepend context path to all jQuery AJAX requests
$.ajaxPrefilter(function( options, originalOptions, jqXHR ) {
    if (!options.crossDomain) {
        options.url = getContextPathWithSlash() + options.url;
    }
});
```
Details: [ajaxPrefilter](http://api.jquery.com/jQuery.ajaxPrefilter/)
