```

<form th:object="${form_model}" class="form" th:action="@{/end_point}" method="post" id="filter_form">

	<div class="selecter">

		<select th:field="*{form_model_field_1}" id="multi_select_1" multiple>
		
			<option th:each="ms1 : ${multi_select_1_list}" th:value="${ms1}" th:text="${ms1}"></option>

		</select> 
		
		
		<select th:field="*{form_model_field_2}" id="multi_select_2" multiple>
		
			<option th:each="ms2 : ${multi_select_2_list}" th:value="${ms2}" th:text="${ms2}"></option>
			
		</select> 
		
		<input id="btn_clear" class="btn btn-success" type="button" value="Clear">

	</div>

</form>
```

## filter_form.js
```
$filter_form = $('#filter_form');
$multi_select_1 = $('#multi_select_1');
$multi_select_2 = $('#multi_select_2');
$btn_clear = $("#btn_clear");

function submit_filter_form() {

	$filter_form.submit();
}

$multi_select_1.select2({

	placeholder : 'Select multi_select_1',
	allowClear : true,
	closeOnSelect : true,
	multiple : true,
	width : '40%',
})

$multi_select_2.select2({

	placeholder : 'Select multi_select_2',
	allowClear : true,
	closeOnSelect : true,
	multiple : true,
	width : '40%',
});

$(document).ready(function() {

	/* listen for select / deselect events */
	$multi_select_1.on('change.select2', function(e) {

		e.preventDefault();

		// console.log(e.params);

		submit_filter_form();
	});

	$multi_select_2.on('change.select2', function(e) {

		e.preventDefault();

		// console.log(e.params);

		submit_filter_form();

	});
	
	

	// clear button
	
	$btn_clear.on("click", function(e) {

		e.preventDefault();

		//console.log('btn_clear');

		$multi_select_1.val(null).trigger('change.select2');

		$multi_select_2.val(null).trigger('change.select2');

	});
});
```
