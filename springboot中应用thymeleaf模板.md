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

    @RequestMapping("/hello")
    public String hello()
    {
        return "index";
    }
```
