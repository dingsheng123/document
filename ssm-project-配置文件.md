具体pom.xml配置：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.ssm.demo</groupId>
	<artifactId>ssm-project</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<!-- spring版本号 -->
		<spring.version>4.3.5.RELEASE</spring.version>
		<!-- log4j日志文件管理包版本 -->
		<slf4j.version>1.7.16</slf4j.version>
		<log4j.version></log4j.version>
		<!-- junit版本号 -->
		<junit.version>4.12</junit.version>
	</properties>

	<dependencies>
		<!-- 添加Spring依赖 -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aop</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-aspects</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-web</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring.version}</version>
			<scope>test</scope>
		</dependency>

		<!-- mybatis 包 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.1</version>
		</dependency>

		<!--mybatis spring 插件 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.3.0</version>
		</dependency>

		<!--单元测试依赖 -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>${junit.version}</version>
			<scope>test</scope>
		</dependency>

		<!-- 日志文件管理包 -->
		<!-- log start -->
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-api</artifactId>
			<version>2.7</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.apache.logging.log4j</groupId>
			<artifactId>log4j-core</artifactId>
			<version>2.7</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${slf4j.version}</version>
		</dependency>
		<!-- log end -->

		<!-- 格式化对象，方便输出日志 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.9</version>
		</dependency>

		<!-- mysql驱动包 -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.29</version>
		</dependency>

		<!-- javaee-api包 注意和项目使用的JDK版本对应 -->
		<dependency>
			<groupId>javax</groupId>
			<artifactId>javaee-api</artifactId>
			<version>6.0</version>
			<scope>provided</scope>
		</dependency>

		<!-- javaee-web-api包 注意和项目使用的JDK版本对应 -->
		<dependency>
			<groupId>javax</groupId>
			<artifactId>javaee-web-api</artifactId>
			<version>6.0</version>
			<scope>provided</scope>
		</dependency>
	</dependencies>

	<build>
		<finalName>ssm-project</finalName>
		<resources>
            <resource>
                <filtering>true</filtering>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
                <excludes>
                    <exclude>log4j.properties</exclude>
                </excludes>
            </resource>
            <resource>
                <filtering>true</filtering>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
            <resource>
                <filtering>true</filtering>
                <directory>src/test/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.5.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>
	
</project>
```



具体配置studentMapping.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.demo.dao.StudentDao">
	<resultMap id="BaseResultMap" type="com.demo.domain.Student">
		<id column="id" property="id" jdbctype="char" />
		<name column="name" property="name" jdbctype="char" />
		<age column="age" property="age" jdbctype="number" />
		<gender column="gender" property="gender" jdbctype="char" />
		<mobile column="mobile" property="mobile" jdbctype="char" />
		<email column="mobile" property="email" jdbctype="char" />
	</resultMap>
	
	<select id="query" parameterType="char" resultMap="BaseResultMap">
		select * from t_student where id = #{id}
	</select>
</mapper>

```



具体配置mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-config.dtd">  
<configuration>    
</configuration>
```



具体配置jdbc.properties

```properties
jdbc_driverClassName=com.mysql.jdbc.Driver
jdbc_url=jdbc:mysql://localhost:3306/student
jdbc_username=root
jdbc_password=root
```



具体配置spring-mvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:p="http://www.springframework.org/schema/p"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xsi:schemaLocation="
    http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-3.2.xsd
    http://www.springframework.org/schema/mvc
    http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd">
    
    
    
    
    <!-- controller层注入 -->
    <context:component-scan base-package="com.demo.controller" />
    
    <!-- 对模型视图添加前后缀 -->
    <bean id="viewResolve" class="org.springframework.web.servlet.view.InternalResourceViewResolver"
    	p:prefix="WEB-INF/jsp/" p:suffix=".jsp" />

</beans>    

```



具体配置application.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xsi:schemaLocation="  
           http://www.springframework.org/schema/beans  
           http://www.springframework.org/schema/beans/spring-beans-3.0.xsd  
           http://www.springframework.org/schema/aop  
           http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
           http://www.springframework.org/schema/context  
           http://www.springframework.org/schema/context/spring-context-3.0.xsd">

	<!-- 引入jdbc配置文件 -->
	<bean id="propertyConfigurer"
		class="org.springframework.beans.factory.config.PropertyPlaceholderConfigurer">
		<property name="localhost">
			<list>
				<value>classpath:properties/*.properties</value>
			</list>
		</property>
	</bean>

	<!-- 配置数据源 -->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName">
			<value>${jdbc_driverClassName}</value>
		</property>
		<property name="url">
			<value>${jdbc_url}</value>
		</property>
		<property name="username">
			<value>${jdbc_username}</value>
		</property>
		<property name="password">
			<value>${jdbc_password}</value>
		</property>
	</bean>
	
	<!-- 自动扫描所有的xxxMapper.xml对应的mapper接口文件。 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage">
			<value>com.demo.dao</value>
		</property>
	</bean>
	
	<!-- 配置mybatis文件，mapperLocations配置xxxMapper.xml文件位置，configLocation配置mybatis-config.xml文件位置 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="mapperLocations" value="classpath*:com/demo/mapper/**/*.xml" />
		<property name="configLocation" value="classpath:mybatis/mybatis-config.xml" />
	</bean>

	<!-- 自动扫描注解的bean -->
	<context:component-scan base-package="com.demo.service" />
	
</beans>
```



具体配置log4j2.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<configruation>
	<!-- 定义appender -->
	<appenders>
	
		<!-- 控制台输出日志 -->
		<console name="STDOUT" target="SYSTEM_OUT">
			<!-- ThresholdFilter控制只输出level以上级别的日志信息，其他的不输出 -->
			<ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
			<PatternLayout pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n" />
		</console>
		
		<!-- 文件形式输出日志 append为true表示后续日志自动追加到本日志文件后面，为false表示，把以前的日志清空再写入新的日志到文件中 -->
		<file name="logFile" fileName="test.log" append="true">
			ThresholdFilter控制只输出level以上级别的日志信息，其他的不输出
			<ThresholdFilter level="debug" onMatch="ACCEPT" onMismatch="DENY"/>
			<PatternLayout pattern="%d %-5p [%t] %C{2} (%F:%L) - %m%n" />
		</file>
		
	</appenders>
	
	<Loggers>
		<!-- 配置记录器级别 -->
		<Root level="debug">
			<!-- 输出设置 -->
			<AppenderRef ref="STDOUT" />
			<AppenderRef ref="logFile" />
		</Root>
	</Loggers>
</configruation>
```

