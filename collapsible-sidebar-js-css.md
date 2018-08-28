## `sidebar.js`
```
function isMenuOpen() {
	return !($("#wrapper").hasClass('toggled'));
}

function setSidebarOpen(flag) {
	$('#sidebarOpen').val(flag);
}

function toggleState() {
	
	let $indicatorArrow = $('#arrow_icon');
	
	if (isMenuOpen()) { // will be closed

		$indicatorArrow.toggleClass("fa-chevron-left fa-chevron-right");
		setSidebarOpen(false);

	} else { // will be opened

		$indicatorArrow.toggleClass("fa-chevron-right fa-chevron-left")
		setSidebarOpen(true);
	}

	$("#wrapper").toggleClass("toggled");
}

$(document).ready((e) => {

	$("#menu-toggle").click(function(e) {
		e.preventDefault();
		toggleState();
	});
});
```

## `sidebar.css`
```
body {
	padding-top: 0px;
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
	right: 0px;
	width: 200px;
	margin: 0;
	padding: 0;
	list-style: none;
}

.sidebar-nav li {
	text-indent: 14px;
	line-height: 40px;
}

.sidebar-nav li a {
	display: block;
	text-decoration: none;
	color: #e2e2e2;
	font-size:13px;
	font-family: Meiryo;
}
.sidebar-nav li a.selected {
	color: #fff;
	background-color: #909090;
}

.sidebar-nav li a:hover {
	text-decoration: none;
	color: #fff;
	background: #312A25;
}
.sidebar-nav li a:hover.selected {
	text-decoration: none;
	color: #fff;
	background-color: #606060;
}

.sidebar-nav li a:active, .sidebar-nav li a:focus {
	text-decoration: none;
}

.sidebar-nav>.sidebar-brand {
	height: 65px;
	font-size: 18px;
	line-height: 0px;
	font-family:meiryo;
	margin-right: 6px;
}

.sidebar-nav>.sidebar-brand a {
	color: #e2e2e2;
}

.sidebar-nav>.sidebar-brand a:hover {
	color: #fff;
	background: none;
}

@media ( min-width :768px) {
	#wrapper {
		padding-left: 130px;
	}
	#wrapper.toggled {
		padding-left: 57px;
	}
	#sidebar-wrapper {
		width: 100px;
	}
	#wrapper.toggled #sidebar-wrapper {
		width: 27px;
	}
	#wrapper.toggled span {
		visibility: visible;
	}
	
	/*
	#wrapper.toggled span.select2-selection.select2-selection--multiple{
		visibility: visible;
	}
	
	#wrapper.toggled span.select2-selection__choice__remove {
		visibility: visible;
	}
	
	#wrapper.toggled #info-section .input-group span  {
		visibility: visible;
	}
	*/
	
	#wrapper.toggled   i#arrow_icon {
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
	#wrapper.toggled   i#arrow_icon {
		float: right;
	}
	#wrapper   i#arrow_icon {
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


```
