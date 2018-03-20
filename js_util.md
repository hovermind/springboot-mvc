## Browser timezone
```
function fetchTimeZoneName(){
	
	let dtf = new Intl.DateTimeFormat();
	
	let ro = dtf.resolvedOptions()
	
	let timeZoneName = ro.timeZone;
	
	return timeZoneName;
}

function pad(value) {
    return value < 10 ? '0' + value : value;
}

function fetchTimeZoneOffSetString() {
	
	let date = new Date();
	
	let sign = (date.getTimezoneOffset() > 0) ? "-" : "+";
	let offset = Math.abs(date.getTimezoneOffset());
	let hours = pad(Math.floor(offset / 60));
	let minutes = pad(offset % 60);
	
    return sign + hours + ":" + minutes;
}

function fetchTimeZoneOffSet(){
	
	let date = new Date();
	
	return date.getTimezoneOffset();
}
```
## Delete cookie
```
function deleteCookie(id, path){
	
	if(!id){
		id = 'JSESSIONID';
	}
	
	if(!path){
		path = '/mc';
	}
	
	Cookies.remove(id, { path: path });
	console.log('attemted to delete cookie for ' + id);
	
	let cookey = Cookies.get(id);
	if(cookey){  // removeCookie did not work
		console.log('removeCookie did not work => trying : Cookies.set');
		Cookies.set(id, '', { expires: -1, path: path });
	}
}
```
## Context path
```
// in template (html head)
<meta name="ctx" th:content="${#httpServletRequest.getContextPath()}" />

function getContextPathWithSlash(){
	
	let ctxPath = $('meta[name="ctxPath"]').attr("content");
	
	if(!ctxPath){
		return '/';
	}
	
	return ctxPath + '/';
}
```
