#### springboot项目中使用log4j2日志框架

##### 1. 引用log4j2
springboot项目中的pom文件中的**spring-boot-starter-web**包中有**spring-boot-starter**引用，默认的日志框架引用在**spring-boot-starter**中，默认的是logback框架。使用log4j2需先排除logback依赖。pom文件中按如下修改：
```
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<exclusions>
				<exclusion>
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-starter-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-log4j2</artifactId>
		</dependency>
```

##### 2. 自定义log4j2
使用自定义的log4j2需要先在application.properties文件中添加引用
> logging.config=classpath:log4j2.xml

在resources目录下，新建log4j2.xml文件，内容及配置解释如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status，这个用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，你会看到log4j2内部各种详细输出-->
<!--monitorInterval：Log4j能够自动检测修改配置 文件和重新配置本身，设置间隔秒数-->
<configuration status="WARN" monitorInterval="30">
    <!-- 日志文件目录和压缩文件 -->
    <Properties>
        <Property name="fileName">/workspace/logs</Property>
        <Property name="fileGz">/workspace/logs/7z</Property>
    </Properties>

    <!--先定义所有的appender-->
    <appenders>
        <!--这个输出控制台的配置-->
        <Console name="Console" target="SYSTEM_OUT">
            <!--输出日志的格式-->
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </Console>

        <!--append为true表示后续日志自动追加到本日志文件后面，为false表示把以前的日志清空再写入新的日志到文件中-->
        <File name="log" fileName="${fileName}/test.log" append="false">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </File>

        <!-- 这个会打印出所有的info及以下级别的信息，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档-->
        <RollingFile name="RollingFileInfo" fileName="${fileName}/file-info.log"
                     filePattern="${fileGz}/$${date:yyyy-MM}/file-info-%d{yyyy-MM-dd}-%i.gz">
            <!--控制台只输出level及以上级别的信息（onMatch），其他的直接拒绝（onMismatch）-->
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} [%t] %-5level %logger{36} %L %M - %msg%xEx%n" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="6" modulate="true" />
                <SizeBasedTriggeringPolicy size="50 MB"/>
            </Policies>
        </RollingFile>

        <!-- 这个会打印出所有的warn及以下级别的信息，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档-->
        <RollingFile name="RollingFileWarn" fileName="${fileName}/file-warn.log"
                     filePattern="${fileGz}/$${date:yyyy-MM}/file-warn-%d{yyyy-MM-dd}-%i.gz">
            <ThresholdFilter level="warn" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} [%t] %-5level %logger{36} %L %M - %msg%xEx%n" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="6" modulate="true" />
                <SizeBasedTriggeringPolicy size="50 MB"/>
            </Policies>
            <!-- DefaultRolloverStrategy属性如不设置，则默认为最多同一文件夹下7个文件，这里设置了20 -->
            <DefaultRolloverStrategy max="20"/>
        </RollingFile>

        <!-- 这个会打印出所有的error及以下级别的信息，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档-->
        <RollingFile name="RollingFileError" fileName="${fileName}/error.log"
                     filePattern="${fileGz}/$${date:yyyy-MM}/file-error-%d{yyyy-MM-dd}-%i.gz">
            <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} [%t] %-5level %logger{36} %L %M - %msg%xEx%n" />
            <Policies>
                <TimeBasedTriggeringPolicy interval="6" modulate="true" />
                <SizeBasedTriggeringPolicy size="50 MB"/>
            </Policies>
            <!-- DefaultRolloverStrategy属性如不设置，则默认为最多同一文件夹下7个文件，这里设置了20 -->
            <DefaultRolloverStrategy max="20"/>
        </RollingFile>
    </appenders>

    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <loggers>
        <!--过滤掉spring和mybatis的一些无用的DEBUG信息-->
        <!--<logger name="org.springframework" level="INFO"></logger>-->
        <!--<logger name="org.mybatis" level="INFO"></logger>-->
        <root level="info">
            <appender-ref ref="Console"/>
            <appender-ref ref="RollingFileInfo"/>
            <appender-ref ref="RollingFileWarn"/>
            <appender-ref ref="RollingFileError"/>
        </root>
    </loggers>
</configuration>
```
