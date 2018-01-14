#### springboot中应用thymeleaf模板
##### 1. pom文件中引用thymeleaf
pom文件添加一下依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

##### 2. application.properties文件添加以下thymeleaf配置项
```
#thymeleaf
spring.thymeleaf.prefix=classpath:/templates/
spring.thymeleaf.suffix=.html
spring.thymeleaf.model=HTML5
spring.thymeleaf.encoding=UTF-8
spring.thymeleaf.content-type=text/html
spring.thymeleaf.cache=false
spring.thymeleaf.chain.strategy.content.enable=true
spring.thymeleaf.chain.strategy.content.path=/**
```

##### 3. 添加静态文件目录
在resources目录下，新建static目录，用于存放静态文件，如 css、js等
在resources目录下，新建templates目录，存放thymeleaf模板文件

##### 4. controller类中应用
```
    @RequestMapping(value = {"/", "/index"})
    public ModelAndView index(ModelAndView modelAndView)
    {
        modelAndView.setViewName("/index");
        modelAndView.addObject("title", "spring boot");
        return modelAndView;
    }
```

##### 4. thymeleaf与bootstrap的整合
将下载好的bootstrap的静态资源存放在resources/static/bootstrap目录下
在application.properties文件中添加对静态资源的引用
```
#设置静态资源的请求路径
spring.mvc.static-path-pattern=/**
#指定静态资源的路径
spring.resources.static-locations=classpath:/static/
```

在新建的html文件中引用bootstrap
```
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org" lang="zh-CN">
<head>
    <meta charset="utf-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1"/>
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>login</title>
    <link rel="stylesheet" th:href="@{bootstrap/css/bootstrap.min.css}"/>
    <link rel="stylesheet" th:href="@{css/login.css}"/>
</head>
<body>
<div class="container">
    <form class="form-signin" th:action="@{/login}" method="post">
        <h2 class="form-signin-heading">Please login</h2>
        <label for="name" class="sr-only">username</label>
        <input type="text" id="name" name="username" class="form-control" placeholder="username" required="true" autofocus="true"/>
        <label for="password" class="sr-only">Password</label>
        <input type="password" id="password" name="password" class="form-control" placeholder="Password" required="true"/>
        <div class="checkbox">
            <label>
                <input type="checkbox" value="remember-me" /> Remember me
            </label>
        </div>
        <button class="btn btn-lg btn-primary btn-block" type="submit">login</button>
    </form>
</div>
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery-3.2.1.min.js"></script>
<script th:src="@{bootstrap/js/bootstrap.min.js}"></script>
</body>
</html>
```
