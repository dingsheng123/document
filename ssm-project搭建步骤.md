1. 新建Maven项目

![1](C:\Users\admin\Desktop\ssm-image\1.png)![2](C:\Users\admin\Desktop\ssm-image\2.png)

![3](C:\Users\admin\Desktop\ssm-image\3.png)

![4](C:\Users\admin\Desktop\ssm-image\4.png)

![5](C:\Users\admin\Desktop\ssm-image\5.png)![6](C:\Users\admin\Desktop\ssm-image\6.png)

![7](C:\Users\admin\Desktop\ssm-image\7.png)







1. 配置pom文件

2. 完成Java Dao层代码的实现（实体类，dao方法，service和impl）

3. 配置mapper文件，和dao文件对应（路径在src/main/java下新建的mapper包下）

4. 整合mybatis和Spring（整合dao层）

   （1）配置jdbc.properties(路径在src/main/resource/properties下)

   （2）配置mybatis的mybatis-config.xml(路径在src/main/resources/mybatis下)

   （3）spring配置：application.xml

   （4）配置日志文件：log4j2.xml

5. 整合mybatis和springMVC(整合controller层)

   （1）创建springmvc.xml,配置处理器映射器、适配器、视图解析器

   （2）配置web.xml

   （3）编写控制器代码