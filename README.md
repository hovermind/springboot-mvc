[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](http://paypal.me/hovermind/10usd)

## Generate Project
Go to: [start.spring.io](http://start.spring.io/) and Generate Project

## pom.xml
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
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>

		<dependency>
			<groupId>org.thymeleaf.extras</groupId>
			<artifactId>thymeleaf-extras-springsecurity4</artifactId>
			<version>3.0.1.RELEASE</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-mongodb</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<optional>true</optional>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>3.2.1</version>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>bootstrap</artifactId>
			<version>4.0.0-beta.3</version>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery-ui</artifactId>
			<version>1.12.1</version>
		</dependency>

		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>font-awesome</artifactId>
			<version>5.0.2</version>
		</dependency>

		<dependency>
			<groupId>com.squareup.retrofit2</groupId>
			<artifactId>retrofit</artifactId>
			<version>2.3.0</version>
		</dependency>

		<dependency>
			<groupId>com.squareup.retrofit2</groupId>
			<artifactId>converter-scalars</artifactId>
			<version>2.1.0</version>
		</dependency>

		<dependency>
			<groupId>com.squareup.retrofit2</groupId>
			<artifactId>converter-jackson</artifactId>
			<version>2.3.0</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>

	</dependencies>

	<build>

		<finalName>hovermind</finalName>

		<plugins>

			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>

			<plugin>
				<artifactId>maven-assembly-plugin</artifactId>
				<configuration>
					<archive>
						<manifest>
							<mainClass>co.jp.softbank.ManagementConsoleApplication</mainClass>
						</manifest>
					</archive>
					<descriptorRefs>jar-with-dependencies</descriptorRefs>
				</configuration>
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


## Single Assembly Build
```
Edit Pom:    
 <build>
  <plugins>
    <plugin>
      <artifactId>maven-assembly-plugin</artifactId>
      <configuration>
        <archive>
          <manifest>
            <mainClass>com.thanga.[REPLACE WITH YOUR MAIN CLASS]</mainClass>
          </manifest>
        </archive>
        <descriptorRefs>
          <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
      </configuration>
    </plugin>
  </plugins>
</build>
```
**Create Run Configuration**    
Name: Pack As Single Assembly    
Goals: `package assembly:single`     

Run (Maven will create two jars - app.jar & depedency.jar)

NB: requires internet connection to download dependencies


## Offline Maven Build
**Offline Build Method 1**
- `mvn dependency:go-offline` command will download everything that your project depends on and will make sure that on a later build, nothing will be downloaded. Then, you can simply make a ZIP of {user.home}/.m2/repository.     
- unzip inside offline machine {user.home}/.m2/repository (will be able to build the project)

**Offline Build Method 2**     
- Get the dependency.jar from "Single Assembly Build" described above    
- Maven command to deploy to offline machine: `mvn install:install-file -Dfile=<path-to-file>`

**[Local Repo](https://gist.github.com/timmolderez/92bea7cc90201cd3273a07cf21d119eb)**

## Thymeleaf instead of JSP
Remove following from `application.properties`    
```
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
```
Jasper dependency is also not needed
```
<dependency>
	<groupId>org.apache.tomcat.embed</groupId>
	<artifactId>tomcat-embed-jasper</artifactId>
	<scope>provided</scope>
</dependency>
```
To use thymeleaf template engine instead of JSP & JSLT, add dependency
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
By default thymeleaf will look in `src/main/resources/templates` with `.html` files (jar deploy)    
To deploy as war change default configuration in `application.properties`     
```
spring.thymeleaf.presfix= /WEB-INF/views/
pring.thymeleaf.suffix= .html
```

## Hot Swapping / Reloading    
Add `spring-boot-devtools` dependency    
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<optional>true</optional>
</dependency>
```
Open `application.properties` set thymeleaf cache to false    
```
spring.thymeleaf.cache= false
```
[DevTools Details](https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html)


## [Thymeleaf master page](https://github.com/hovermind/springboot-webmvc/blob/master/master_page.md)    

## [Model binding](https://github.com/hovermind/springboot-webmvc/blob/master/thymeleaf_model_binding.md)

## [Getting & setting session value](https://github.com/hovermind/springboot-webmvc/blob/master/getting_setting_session_value.md)

## [Custom Error Page](https://github.com/hovermind/springboot-webmvc/blob/master/error_page.md)

## [Controller Advice to Handle Exception & Error](https://github.com/hovermind/springboot-webmvc/blob/master/controller_advice.md)

## [Ajax Call](https://github.com/hovermind/springboot-webmvc/blob/master/ajax_call.md)

## [Configuration Properties and Message](https://github.com/hovermind/springboot-webmvc/blob/master/configuration_properties_and_message.md)

## [Logging with SLF4J and Logback](https://github.com/hovermind/springboot-webmvc/blob/master/logging.md)
