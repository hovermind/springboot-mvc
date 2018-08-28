## Dependency
* JQuery
* Bootstrap
* Font-awesome
* [`sidebar.js` & `sidebar.css`](https://github.com/hovermind/springboot-webmvc/blob/master/collapsible-sidebar-js-css.md)

## `sidebar.html`
```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">

<title>Collapsible Sidebar</title>

<link rel="stylesheet" href="css/bootstrap.css" />
<link rel="stylesheet" href="css/font-awesome/css/font-awesome.css" />
<link rel="stylesheet" href="css/sidebar.css" />

<script type="text/javascript" src="js/jquery.js"></script>
<script type="text/javascript" src="js/bootstrap.js"></script>
<script type="text/javascript" src="js/sidebar.js"></script>

</head>

<body style="width: max-content; overflow: auto;">

	<div class="container">

			<div id="wrapper">

				<div id="sidebar-wrapper">

					<ul class="sidebar-nav" style="margin-left: 0;">

						<li class="sidebar-brand">
							<a href="#menu-toggle" id="menu-toggle" style="margin-top: 20px; float: right;">
								<i id="arrow_icon" class="fa fa-chevron-left" style="font-size: 20px !Important;" aria-hidden="true"></i>
							</a>
						</li>

						<li>
							<a href="#" >
								<span style="margin-left: 90px;">Menu 1</span>
							</a>
						</li>
						
						<li>
							<a href="#" >
								<span style="margin-left: 90px;">Manu 2</span>
							</a>
						</li>
						
					</ul>

				</div>


				<div id="page_content" style="position: absolute;">

					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					<h1>The quick brown fox jumped over the lazy dog</h1>
					
				</div>

			</div>

	</div>

</body>
</html>
```

## Retaining Sidebar Open/Close State
#### Sidebar State Flag in BaseViewModel
Add `sidebarOpen` flag in BaseViewModel (inherited by all viewModels)
```
public class BaseViewModel {

	private String sidebarOpen = true;

	public String isSidebarOpen() {
		return sidebarOpen;
	}

	public void setIsSidebarOpen(String sidebarOpen) {
		this.sidebarOpen = sidebarOpen;
	}
}

public class MyViewModel extends BaseViewModel {
	// ... ... ...
}
```
#### A Hidden Field to Retain Sidebar Open State
A hidden field is bind to `sidebarOpen` property of `BaseViewModel` to retain menu state. 
Value of hidden field is set in `toggleState()` (from `sidebar.js`)
```
<input type="hidden" th:field="{viewModel.sidebarOpen}" />
```

#### Restore Previous State (Opened/Closed)
```
<div id="wrapper" th:class="${viewModel?.sidebarOpen} ? 'toggled'">
```
Synchronize arrow symbol
```
<li class="sidebar-brand">
	<a href="#menu-toggle" id="menu-toggle" style="margin-top: 20px; float: right;">
		<i id="arrow_icon" th:class="${viewModel?.sidebarOpen} ? 'fa fa-chevron-right' : 'fa fa-chevron-left'" style="font-size: 20px !Important;" aria-hidden="true"></i>
	</a>
</li>
```
