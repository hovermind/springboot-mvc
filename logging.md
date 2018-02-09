
## Log Level Setting
`application.xml`
```
#logging.level.root= warn
logging.level.org.springframework= error
logging.level.mypackage= debug
```

## logback-spring.xml
Spring boot already have logback & slf4j dependency.    
Set log folder in `application.xml`
```
# my logging
myprefix.log.folder= C:/myfolder
```
To activate logback, create `logback-spring.xml` (in resource folder)
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

	<include resource="org/springframework/boot/logging/logback/defaults.xml" />

	<springProperty name="LOG_FOLDER" source="myprefix.log.folder" scope="context"/>

	<property name="LOG_FILE" value="${LOG_FOLDER}/my_log_file_name.log" />

	<property name="FILE_LOG_PATTERN" value="%d{yyyy-MM-dd HH:mm:ss} %-5level %logger{0}.%method[%line] --- %message%n" />

	<springProfile name="dev">

		<include
			resource="org/springframework/boot/logging/logback/console-appender.xml" />

		<appender name="ROLLING-FILE"
			class="ch.qos.logback.core.rolling.RollingFileAppender">

			<encoder>
				<pattern>${FILE_LOG_PATTERN}</pattern>
			</encoder>

			<file>${LOG_FILE}</file>

			<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
				<fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}.log</fileNamePattern>
			</rollingPolicy>

		</appender>

		<root level="ERROR">
			<appender-ref ref="CONSOLE" />
			<appender-ref ref="ROLLING-FILE" />
		</root>

	</springProfile>

	<springProfile name="production">

		<appender name="ROLLING-FILE"
			class="ch.qos.logback.core.rolling.RollingFileAppender">

			<encoder>
				<pattern>${FILE_LOG_PATTERN}</pattern>
			</encoder>

			<file>${LOG_FILE}</file>

			<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">

				<fileNamePattern>${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz</fileNamePattern>

				<timeBasedFileNamingAndTriggeringPolicy
					class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
					<maxFileSize>10MB</maxFileSize>
				</timeBasedFileNamingAndTriggeringPolicy>

			</rollingPolicy>

		</appender>

		<root level="ERROR">
			<appender-ref ref="ROLLING-FILE" />
		</root>

	</springProfile>

</configuration>
```

## SLF4J Logging
```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class BaseService {

	protected final Logger myLogger = LoggerFactory.getLogger(this.getClass().getSimpleName());


}

public MyService extends BaseService{

	myMethod(){
		
		// ... ... ...
		
		myLogger.error("Error occurred"); // FILE_LOG_PATTERN in logback-spring.xml will determine log pattern
	}

}
```
