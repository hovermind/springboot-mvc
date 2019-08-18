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
