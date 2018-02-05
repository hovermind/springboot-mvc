## Multiselect by [select2.js](https://select2.org/)
Download js & css from [select2.org](https://select2.org/)    

Custom JS
```
function submit_form(selectFlag) {

	$('#filter_form').submit();
}

$('#a_multiselect').select2({

	placeholder : 'Select a_multiselect',
	allowClear : true,
	closeOnSelect : true,
	multiple : true,
	width : '40%',
})

$('#b_multiselect').select2({

	placeholder : 'Select b_multiselect',
	allowClear : true,
	closeOnSelect : true,
	multiple : true,
	width : '40%',
});

$(document).ready(function() {
	
	$aMultiselect = $('#a_multiselect');
	$bMultiselect = $('#b_multiselect');
	$btnClear = $("#btn_clear");

	/* listen for select / deselect event */
	$aMultiselect.on('change.select2', function(e) {

		e.preventDefault();

		// console.log(e.params);

		submit_form('a'); 
	});

	$bMultiselect.on('change.select2', function(e) {

		e.preventDefault();

		// console.log(e.params);

		submit_form('b'); 

	});

	// clear button
	$btnClear.on("click", function(e) {

		e.preventDefault();
		
		$a = $('#a_multiselect');
		$b = $('#b_multiselect');
		
		$a_array = $a.val();
		$b_array = $b.val();
		if(($a_array == null &&  $b_array == null) || !($a_array.length > 0  || $b_array.length > 0)){
			return; // nothing to clear
		}

		//console.log('btn_clear');

		$a.val(null).trigger('change.select2');

		$b.val(null).trigger('change.select2');

	});
	
});
```
