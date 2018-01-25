## Thymeleaf master template
```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head>

<meta charset="utf-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title th:text="${pageTitle}"></title>

<link th:href="@{/bootstrap-3.3.7/css/bootstrap.min.css}" rel="stylesheet" />
<link th:href="@{/font-awesome-4.7.0/css/font-awesome.min.css}" rel="stylesheet" />

<link th:href="@{/css/custom_master_collapsible_menu.css}" rel="stylesheet" />
<link th:href="@{/css/custom_master_top_bar.css}" rel="stylesheet" />

<script type="text/javascript" th:src="@{/webjars/jquery/3.2.1/jquery.min.js}"></script>
<script type="text/javascript" th:src="@{/bootstrap-3.3.7/js/bootstrap.min.js}"></script>

</head>

<body>

	<div id="top_bar" class="navbar navbar-default navbar-fixed-top" style="margin: 0px">
    					<h2 class="text-center" th:text="${'Spring - ' + pageTitle}" style="margin: 0px; padding: 10px;"> </h2>
	</div>
	
	<div class="container">
	
		<div class="row">
					
			<div id="wrapper">
			
				<div id="sidebar-wrapper">
				
					<ul class="sidebar-nav" style="margin-left: 0;">
			
						<li class="sidebar-brand">
						
							<a href="#menu-toggle" id="menu-toggle" style="margin-top: 20px; float: right;"> 
								<i id="arrow_icon" class="fa fa-chevron-left" style="font-size: 20px !Important;" aria-hidden="true"></i>
							</a>
							
						</li>
						
	             		<li>
	                    	<a th:href="@{/treasury}"> <i class="fa fa-th" aria-hidden="true"> </i> <span style="margin-left:10px;">Treasury</span></a>
	                	</li>
	
	             		<li>
	                    	<a th:href="@{/subscriber}"> <i class="fa fa-bar-chart-o" aria-hidden="true"> </i> <span style="margin-left:10px;">Subscriber</span> </a>
	                	</li>
		
					</ul>
					
				</div>
				
				
				<div id="page_main_content">
				
					<div layout:fragment="content" class="container-fluid"></div>
					
				</div>
				

				
			</div>

		</div>

	</div>

	<script>
	
	
		var isRight = false;
		
		function toggleDirection(){
			
			if(isRight){
				
				isRight = false;
				
			}else{
				
				isRight = true;
				
			}
		}
	
		$("#menu-toggle").click(function(e) {
			
			e.preventDefault();
			
			toggleDirection();
			
			$("#wrapper").toggleClass("toggled");
			
			if(isRight){
				$("#arrow_icon").toggleClass("fa-chevron-left fa-chevron-right")
			}else{
				$("#arrow_icon").toggleClass("fa-chevron-right fa-chevron-left")
			}
		
		});
		


		
	</script>
	
</body>
</html>
```
## Css for side bar collapsible menu
```


body {
	overflow-x: hidden;
}

/* Toggle Styles */
#wrapper {
	padding-left: 0;
	-webkit-transition: all 0.6s ease;
	-moz-transition: all 0.6s ease;
	-o-transition: all 0.6s ease;
	transition: all 0.6s ease;
}

#wrapper.toggled {
	padding-left: 200px;
}

#sidebar-wrapper {
	z-index: 1000;
	position: fixed;
	left: 250px;
	width: 0;
	height: 100%;
	margin-left: -250px;
	overflow-y: auto;
	background-color: #312A25 !Important;
	-webkit-transition: all 0.5s ease;
	-moz-transition: all 0.5s ease;
	-o-transition: all 0.5s ease;
	transition: all 0.5s ease;
}

#wrapper.toggled #sidebar-wrapper {
	width: 0;
}

#page-content-wrapper {
	width: 100%;
	position: absolute;
	padding: 10px;
}

#wrapper.toggled #page-content-wrapper {
	position: absolute;
	margin-left: -250px;
}

/* Sidebar Styles */
.sidebar-nav {
	position: absolute;
	top: 0;
	right: 15px;
	width: 200px;
	margin: 0;
	padding: 0;
	list-style: none;
}

.sidebar-nav li {
	text-indent: 20px;
	line-height: 40px;
}

.sidebar-nav li a {
	display: block;
	text-decoration: none;
	color: #999999;
}

.sidebar-nav li a:hover {
	text-decoration: none;
	color: #fff;
	background: #312A25;
}

.sidebar-nav li a:active, .sidebar-nav li a:focus {
	text-decoration: none;
}

.sidebar-nav>.sidebar-brand {
	height: 65px;
	font-size: 18px;
	line-height: 60px;
}

.sidebar-nav>.sidebar-brand a {
	color: #999999;
}

.sidebar-nav>.sidebar-brand a:hover {
	color: #fff;
	background: none;
}

@media ( min-width :768px) {
	#wrapper {
		padding-left: 250px;
	}
	#wrapper.toggled {
		padding-left: 0;
	}
	#sidebar-wrapper {
		width: 200px;
	}
	#wrapper.toggled #sidebar-wrapper {
		width: 40px;
	}
	#wrapper.toggled span {
		visibility: hidden;
	}
	
	#wrapper.toggled span.select2-selection.select2-selection--multiple{
		visibility: visible;
	}
	
	#wrapper.toggled   i {
		float: right;
	}
	#page-content-wrapper {
		padding: 20px;
		position: relative;
	}
	#wrapper.toggled #page-content-wrapper {
		position: relative;
		margin-right: 0;
	}
	
	body {
		padding-top: 0px;
	}
}

@media ( max-width :414px) {
	#wrapper.toggled #page-content-wrapper {
		position: absolute;
		margin-right: 60px;
	}
	#wrapper.toggled {
		padding-right: 60px;
	}
	#wrapper {
		padding-left: 20px;
	}
	#wrapper.toggled {
		padding-left: 0;
	}
	#sidebar-wrapper {
		width: 50px;
	}
	#wrapper.toggled #sidebar-wrapper {
		width: 140px;
	}
	#wrapper.toggled span {
		visibility: visible;
		position: relative;
		left: 70px;
		bottom: 13px;
	}
	#wrapper span {
		visibility: hidden;
	}
	#wrapper.toggled   i {
		float: right;
	}
	#wrapper   i {
		float: right;
	}
	#page-content-wrapper {
		padding: 5px;
		position: relative;
	}
	#wrapper.toggled #page-content-wrapper {
		position: relative;
		margin-right: 0;
	}
	html body div #top_bar h1 {
		margin: 0px;
	}
}

body {
	padding-top: 50px;
}

```
## Css for fixed top bar
```
#top_bar.navbar-fixed-top{
	//paddingt: 10px;
	background-color: #5555FF
}

#top_bar.navbar-fixed-top h2{
	color: #FFF;
}

#page_main_content{
	padding: 10px;
}
```
