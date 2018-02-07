## Ajax call
```
activityStatus = 0;

function hardReload() {
	window.location.reload(true);
}

function decideReload() {

	console.log("Decide reload - activity status == " + activityStatus);
	
	var temp = activityStatus;
	
	if (activityStatus != 0) {
		activityStatus = 0;
	}

	if (temp == 1) {
		// refresh
		hardReload();
	}
}

$btnTest = $('#btn_test');
$btnTest.click(function(e) {

	e.preventDefault();

	// validation check
	var id = $('#test_id').val();
	var day = $('#test_day_select option[selected=selected]').val();

	if (id == '') {
		showMessage("Enter Id");
		return;
	}

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
	
		activityStatus = 1;
		
		//onAjaxSucess(data);

	}).fail(function(error) {
	
		activityStatus = -1;
	
		//onAjaxFailure(error);
		
	}).always(function(){
	
		decideReload();
	
	});
});
```
