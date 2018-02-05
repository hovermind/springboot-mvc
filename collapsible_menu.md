


## Collapsible Menu

Dependency : JQuery, Bootstrap     

[Github: sidr](https://github.com/artberri/sidr)    
[Doc: sidr](https://www.berriart.com/sidr/)    

Custom JavaScript
```
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
```
