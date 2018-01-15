## Generate Project
Go to: [start.spring.io](http://start.spring.io/) and Generate Project

## Maven pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	
	<modelVersion>4.0.0</modelVersion>
	
	<!-- ***************************************** project ***************************************** -->

	<groupId>com.hovermind</groupId>
	<artifactId>SpringBootWebMvcDemo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>Spring Boot Web Mvc Demo App</name>
	<description>Demo Web MVC project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.9.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<!-- ***************************************** dependencies ***************************************** -->
	
	<dependencies>
	
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
			<scope>provided</scope>
		</dependency>

	</dependencies>

	<!-- ***************************************** build & plugins ***************************************** -->
	
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>


</project>
```

## Application Class
```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication // ~ @Configuaretion + @EnableAutoConfiguration + @ComponentScan ()
public class Application {

	public static void main(String[] args) {
	
		SpringApplication.run(Application.class, args);
		
		//System.out.println("Spring Boot Web MVC Started...");
	}
}
```

## application.properties
Go to: src/main/resources > application.properties   
Add:
```
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```

## Controller
- Create controller package   
- Create controller
```
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@RequestMapping("/test")
public class TestController {

	
	@GetMapping("/")
	public String dsiplayTestPage(Model model) {
		
		model.addAttribute("tm", "Test Message");
		
		return "test";
	}
}
```

## Run Configuration
**Create Run Configuration:**  
Name: Mvn Spring Boot Run    
Goals: spring-boot:run    

Alternative:    
Right click on project > Run As > Spring Boot App    

**Cleaning**   
Name: Mvn Clean     
Goals: clean    

