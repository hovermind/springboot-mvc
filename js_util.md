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
