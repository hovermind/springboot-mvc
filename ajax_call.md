## Ajax call
```
// json payload
var jsonOjb = {};
jsonOjb["id"] = id;
jsonOjb["day"] = day;
jsonData = JSON.stringify(jsonOjb);

// submit form
$.ajax('api/v1/test', {
	type : 'POST',
	contentType : 'application/json', // send as
	dataType : 'json', // return as
	data : jsonData
}).done(function(data) {

    //onAjaxSucess(data);

}).fail(function(error) {

    //onAjaxFailure(error);

}).always(function(){
    // ....
});
```
