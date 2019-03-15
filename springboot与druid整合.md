#### 1. springboot 与 druid的整合

##### 1. 添加依赖
在pom文件中添加druid依赖
```
<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.29</version>
		</dependency>
```

##### 2. 添加配置项
在 application.properties文件中添加引用
```
#数据库配置
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/mydatabase
spring.datasource.username=root
spring.datasource.password=root

#数据库连接池配置
#初始化大小，最小，最大
spring.datasource.initialSize=5
spring.datasource.minIdle=5
spring.datasource.maxActive=20
#配置获取连接等待超时时间(毫秒)
spring.datasource.maxWait=60000
#配置间隔多久才进行一次检测，检测需要关闭的空闲连接（毫秒）
spring.datasource.timeBetweenEvictionRunsMillis=60000
#配置在一个连接池中最小生存时间（毫秒）
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=SELECET 1 FROM DUAL
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
#打开PSCache，并且指定每个连接上PSCache的大小
spring.datasource.poolPreparedStatements=true
spring.datasource.maxPoolPreparedStatementPerConnectionSize=20
#配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
spring.datasource.filters=stat,wall,log4j2
#通过connectProperties属性来打开mergeSql功能；慢sql记录
spring.datasource.connectionProperties=druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000
#合并多个DruidDataSource的监控数据
#spring.datasource.useGlobalDataSourceStat=true
```

##### 3. 在项目中手动添加druid的相关配置
1. 添加druid参数对象配置DruidParam.java
```
package com.dingsheng.configuration.druid;

import org.springframework.boot.context.properties.ConfigurationProperties;

/**
 * Druid参数
 *
 * @author dingsheng
 * @create 2018-01-19 16:22
 **/
@ConfigurationProperties(prefix = "spring.datasource")
public class DruidParam
{
    private String type;
    private String driverClassName;
    private String url;
    private String username;
    private String password;

    private Integer initialSize;
    private Integer minIdle;
    private Integer maxActive;

    private Long maxWait;
    private Long timeBetweenEvictionRunsMillis;
    private Long minEvictableIdleTimeMillis;

    private String validationQuery;
    private boolean testWhileIdle;
    private boolean testOnBorrow;
    private boolean testOnReturn;

    private boolean poolPreparedStatements;
    private Integer maxPoolPreparedStatementPerConnectionSize;
    private String filters;
    private String connectionProperties;
    private boolean useGlobalDataSourceStat;

}
```

2. DruidDataSource配置 DruidConfiguration.Java
```
package com.dingsheng.configuration;

import com.alibaba.druid.pool.DruidDataSource;
import com.dingsheng.configuration.druid.DruidParam;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.jdbc.DataSourceProperties;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.sql.SQLException;
import java.util.Properties;

/**
 * Druid连接池配置
 *
 * @author dingsheng
 * @create 2018-01-19 16:18
 **/
@Configuration
@EnableConfigurationProperties(DruidParam.class)
public class DruidConfiguration
{
    /**
     * 日志
     */
    private static final Logger LOGGER = LoggerFactory.getLogger(DruidConfiguration.class);

    @Autowired
    private DruidParam druidParam;

    @Bean
    @ConfigurationProperties("application.properties")
    public DruidDataSource dataSource(DataSourceProperties properties) throws SQLException
    {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(druidParam.getDriverClassName());
        dataSource.setUrl(druidParam.getUrl());
        dataSource.setUsername(druidParam.getUsername());
        dataSource.setPassword(druidParam.getPassword());
        dataSource.setInitialSize(druidParam.getInitialSize());
        dataSource.setMinIdle(druidParam.getMinIdle());
        dataSource.setMaxActive(druidParam.getMaxActive());
        dataSource.setMaxWait(druidParam.getMaxWait());
        dataSource.setTimeBetweenEvictionRunsMillis(druidParam.getTimeBetweenEvictionRunsMillis());
        dataSource.setMinEvictableIdleTimeMillis(druidParam.getMinEvictableIdleTimeMillis());
        String validationQuery = druidParam.getValidationQuery();
        if (validationQuery != null && !"".equals(validationQuery)) {
            dataSource.setValidationQuery(validationQuery);
        }
        dataSource.setTestWhileIdle(druidParam.isTestWhileIdle());
        dataSource.setTestOnBorrow(druidParam.isTestOnBorrow());
        dataSource.setTestOnReturn(druidParam.isTestOnReturn());
        if(druidParam.isPoolPreparedStatements()){
            dataSource.setMaxPoolPreparedStatementPerConnectionSize(druidParam.getMaxPoolPreparedStatementPerConnectionSize());
        }
        dataSource.setFilters(druidParam.getFilters());//这是最关键的,否则SQL监控无法生效
        String connectionPropertiesStr = druidParam.getConnectionProperties();
        if(connectionPropertiesStr != null && !"".equals(connectionPropertiesStr)){
            Properties connectProperties = new Properties();
            String[] propertiesList = connectionPropertiesStr.split(";");
            for(String propertiesTmp:propertiesList){
                String[] obj = propertiesTmp.split("=");
                String key = obj[0];
                String value = obj[1];
                connectProperties.put(key,value);
            }
            dataSource.setConnectProperties(connectProperties);
        }
        dataSource.setUseGlobalDataSourceStat(druidParam.isUseGlobalDataSourceStat());

        return dataSource;

    }
}

```

3. 添加filter  DruidStatFilter.Java
```
package com.dingsheng.filter;

import com.alibaba.druid.support.http.WebStatFilter;

import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;

/**
 * TODO
 *
 * @author dingsheng
 * @create 2018-01-19 16:48
 **/
@WebFilter(filterName="druidStatFilter",urlPatterns="/*",
        initParams={
                @WebInitParam(name="exclusions",value="*.js,*.gif,*.jpg,*.bmp,*.png,*.css,*.ico,/druid/*")// 忽略资源
        })
public class DruidStatFilter extends WebStatFilter
{

}
```

4. 添加servlet
```
package com.dingsheng.servlet;

import com.alibaba.druid.support.http.StatViewServlet;

import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;

/**
 * TODO
 *
 * @author dingsheng
 * @create 2018-01-19 16:45
 **/
@SuppressWarnings("serial")
@WebServlet(urlPatterns = "/druid/*",
        initParams={
                @WebInitParam(name="allow",value="127.0.0.1"),// IP白名单 (没有配置或者为空，则允许所有访问)
                @WebInitParam(name="deny",value=""),// IP黑名单 (存在共同时，deny优先于allow)
                @WebInitParam(name="loginUsername",value="root"),// 用户名
                @WebInitParam(name="loginPassword",value="root"),// 密码
                @WebInitParam(name="resetEnable",value="false")// 禁用HTML页面上的“Reset All”功能
        })
public class DruidStatViewServlet extends StatViewServlet
{
}

```

可以访问http://localhost:8080/druid/index.html查看效果
