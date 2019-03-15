#### springboot中使用mybatis

##### 1. 添加依赖
在pom文件中添加以下依赖
```
    <dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.3.0</version>
		</dependency>

		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>

		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.29</version>
		</dependency>
```

##### 2.修改配置文件
在application.properties文件中添加引用
```
#mybatis配置
mybatis.mapper-locations=classpath:mapper/*.xml
```

#### 3. mysql建表
新建一个简单的表t_user_info
```
create table t_user_info
(
  username char(20) not null,
  sex char(5) not null
  );

  insert into t_user_info valuse ("xiaoming", 20);
```

##### 4. 新建user实体类  UserEntity
```
package com.dingsheng.entity;

/**
 * TODO
 *
 * @author dingsheng
 * @create 2018-01-19 17:20
 **/
public class UserEntity
{
    private String username;

    private String sex;

    public String getUsername()
    {
        return username;
    }

    public void setUsername(String username)
    {
        this.username = username;
    }

    public String getSex()
    {
        return sex;
    }

    public void setSex(String sex)
    {
        this.sex = sex;
    }

    @Override
    public String toString()
    {
        return "UserEntity{" +
                "username='" + username + '\'' +
                ", sex='" + sex + '\'' +
                '}';
    }
}

```

##### 5. 新建mapper类   UserMapper
```
package com.dingsheng.mapper;

import com.dingsheng.entity.UserEntity;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;

/**
 * TODO
 *
 * @author dingsheng
 * @create 2018-01-19 17:19
 **/
@Mapper
public interface UserMapper
{
    UserEntity getUserInfo(String username);

    @Select("select sex from t_user_info where username = #{username}")
    String getSex(@Param("username") String username);
}

```

##### 6. 新建mapper.xml文件
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.dingsheng.mapper.UserMapper">

    <resultMap id="userinfo"
               type="com.dingsheng.entity.UserEntity">
        <result property="username" column="username"/>
        <result property="sex" column="sex"/>
    </resultMap>

    <select id="getUserInfo" parameterType="java.lang.String" resultMap="userinfo">
        select * from t_user_info where username=#{username};
    </select>
</mapper>
```

##### 7. 新建controller类进行测试
```
package com.dingsheng.controller;

import com.dingsheng.entity.UserEntity;
import com.dingsheng.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * TODO
 *
 * @author dingsheng
 * @create 2018-01-19 17:27
 **/
@RestController
public class UserController
{
    @Autowired
    private UserMapper userMapper;

    @RequestMapping("/userinfo")
    public UserEntity getUserInfo()
    {
        UserEntity entity = userMapper.getUserInfo("xiaoming");
        return entity;
    }

    @RequestMapping("/sex")
    public String getSex()
    {
        String sex = userMapper.getSex("xiaoming");
        return sex;
    }
}

```

##### 7. 在浏览器上测试
输入一下两个网址进行测试：
http://localhost:8080/userinfo
http://localhost:8080/sex

可以通过http://localhost:8080/druid/sql.html 查看sql调用情况


以上配套了注解和xml文件两种方式使用mybatis.实际使用中项目较小，sql简单时使用注解方式比较简便。
实际项目较大，sql复杂时使用xml方式更合理。
