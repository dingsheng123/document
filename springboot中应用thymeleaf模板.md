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

在新建的html文件中引用bootstrap，一个简单的登陆页面
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

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <!--[if lt IE 9]>
    <script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>
    <script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
    <![endif]-->

    <style>
        .col-center-block {
            float: none;
            display: block;
            margin-left: auto;
            margin-right: auto;
        }

        .container {
            display: table;
            height: 100%;
            margin-top: 200px;
        }

        .row {
            display: table-cell;
            vertical-align: middle;
        }

    </style>
</head>
<body th:style="'background:url('+ @{images/012.png} +'); background-size:100%;'">
<div class="container">
    <div class="row">
        <div class="well col-xs-6 col-md-4 col-center-block">
            <form class="form-horizontal" th:action="@{/login}" th:method="post">
                <h3 class="form-signin-heading">请登录</h3>
                <label for="username" class="sr-only">用户名</label>
                <input type="text" id="username" class="form-control" placeholder="用户名" required="true"
                       autofocus="true"/>
                <label for="inputPassword" class="sr-only" style="margin-top: 20px">密码</label>
                <input type="password" id="inputPassword" style="margin-top: 20px" class="form-control" placeholder="密码"
                       required="true"/>
                <div class="checkbox" style="margin-top: 10px">
                    <label>
                        <input type="checkbox" value="remember-me"/>记住我
                        <a href="#" style="margin-left: 220px">注册</a>
                    </label>
                </div>
                <button class="btn btn-lg btn-primary btn-block" type="submit" style="margin-top: 15px">登录</button>
            </form>
        </div>
    </div>
</div>
<script src="https://cdn.bootcss.com/jquery/1.12.4/jquery-3.2.1.min.js"></script>
<script th:src="@{bootstrap/js/bootstrap.min.js}"></script>
</body>
</html>
```
