
## Setting value in session

```
@Controller
@SessionAttributes("my_data")
public class TreasuryController {

	/*
	* will be called (once per session) before any request handler
	* returned value will be set in session with key "my_data"
	*/
	@ModelAttribute("my_data")  
	private Data populateData() {
		
		Data data = new Data();
		
		// get data from repository or api call
		// data = ...

		return data;
	}
}
```

## Getting value from session
```
@GetMapping("/end_point") or @PostMapping("/end_point")
public String MyPagePOST(@ModelAttribute("my_data") Data data, Model model, MyViewModel viewModel) {

	// use data
	
	// viewModel.data = data
	// model.addAttribute("VIEW_MODEL", viewModel);

}
```
