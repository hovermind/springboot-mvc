## FormModel.java    
```
public class FormModel {

	public List<String> selectedItems;

	public List<String> getSelectedItems() {
		return selectedItems;
	}

	public void setSelectedItems(List<String> selectedItems) {
		this.selectedItems = selectedItems;
	}
}
```
## MyController.java    
```
@Controller
public class MyController{

	@GetMapping("/end_point")
	public String displayMyPageGET(Model model) {
		
		model.addAttribute("page_title", "MyPage");
		
		FormModel formModel = new FormModel();
		
		// set form model
		model.addAttribute("form_model", formModel);
		
		// list to pass for multi select
		
		// data to pass for page

		return "mypage";
	}

	@PostMapping("/end_point")
	public String displayMyPagePOST(@ModelAttribute FormModel formModel, Model model) {
		
		model.addAttribute("page_title", "MyPage");
		
		// get & set form model to save state => model binding will handle it correctly
		model.addAttribute("form_model", formModel);

		// list to pass for multi select
		
		// data to pass for page

		return "mypage";
	}
}
```

## mypage.html    
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
Filter.js for above form is [here](https://github.com/hovermind/springboot-webmvc/blob/master/multiselect_select2js.md)    

## Form Model validation    
```
public class FormModel {

	@NotNull
	@Size(min=5, max=255)
	public String firstName;
	
	// ... ... ... ... ...
}

@PostMapping("/end_point")
public String displayMyPagePOST(@Valid FormModel formModel, Model model) {

}


```
