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
    					<h2 class="text-center" th:text="${'Management Console - ' + pageTitle}" style="margin: 0px; padding: 10px;"> </h2>
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
