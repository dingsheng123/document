### Log4j 2.0的使用



#### 日志级别大小关系

> trace < debug < info <warn < error < fatal

只打印比指定日志级别高的日志，如：指定级别日志是info，则只打印info，warn，error，和fatal级别的日志，不打印trace和debug级别的日志。



#### log4j2的配置文件有四种

1. 通过一个配置文件编写的xml、json、或yaml
2. 以编程方式，通过创建一个ConfigruationFactory和配置实现
3. 以编程方式，通过调用api暴露在配置界面添加组件的默认配置
4. 以编程方式，通过调用Logger类内部的方法

**ps** 在log4j 2.x版本中，如果不提供配置文件，它会默认打印error级别的日志。

#### xml配置文件

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration status="error">
    <!--先定义所有的appender-->
    <appenders>
    
        <!--这个输出控制台的配置-->
        <Console name="Console" target="SYSTEM_OUT">
            <!--ThresholdFilter控制只输出level及以上级别的信息，其他的直接拒绝-->
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <!--这个都知道是输出日志的格式-->
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </Console>
        
        <!--append为true表示后续日志自动追加到本日志文件后面，为false表示，把以前的日志清空再写入新的日志到文件中-->
        <File name="log" fileName="log/test.log" append="false">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </File>

        <!--这个会打印出所有的信息，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档-->
        <RollingFile name="RollingFile" fileName="logs/app.log"
                     filePattern="log/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
            <PatternLayout pattern="%d{yyyy-MM-dd 'at' HH:mm:ss z} %-5level %class{36} %L %M - %msg%xEx%n"/>
            <SizeBasedTriggeringPolicy size="50MB"/>
        </RollingFile>
    </appenders>
    
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <loggers>
        <!--建立一个默认的root的logger-->
        <root level="trace">
            <appender-ref ref="RollingFile"/>
            <appender-ref ref="Console"/>
            <appender-ref ref="log"/>
        </root>

    </loggers>
</configuration>
```



#### Java引用

```
private static final Logger LOG = LogManager.getLogger(StudentDaoTest.class);
```

