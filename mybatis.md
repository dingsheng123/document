### mybatis



每个基于 mybaits 的应用都是以一个 **SqlSessionFactoey** 的实例为中心的。

**SqlSessionFactory** 的实例可以通过 **SqlSessionFactoryBuilder** 获得。而 SqlSessionFactoryBuilder 则可以从XML 配置文件或一个预先定制的 onfiguration 实例构建出 SQLSessionFactory 实例。



#### SqlSessionFactoryBuilder

这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。因此， SqlSessionFactoryBuilder 的最佳作用域是方法作用域（也就是局部方法变量）。

可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但是最好不要让其一直存在以保证所有的 XML 解析资源开放给更重要的资源。

#### SqlSessionFactory

SqlSessionFactory 一旦被创建就应该在应用运行期间一直存在，没有任何理由对它进行清楚或重建。使用 SqlSessionFactory 的最佳实践是在应用程序运行期间不要重复创建多次。因此，SQLSessionFactory 的最佳作用域是应用作用域。可以通过单例模式或者静态单例模式实现。

#### SqlSession

每个线程都应该有自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳作用域是请求或方法作用域。绝不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。也绝不能将SqlSession实例的引用放在任何类型的管理作用域中，比如 Servlet 架构中的 HttpSession。换句话说，每次收到的 HTTP 请求，就可以打开一个 SqlSession ，返回一个响应，就关闭它。



#### mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- 从外部指定properties配置文件 -->
    <properties resource="mybatis/jdbc.properties" />
    
    <!-- environments的default属性指定应用哪一个environment -->
    <environments default="test">
    
        <environment id="development">
            <!-- 
            JDBC     这个配置直接简单使用了JDBC的提交和回滚设置，依赖与从数据源得到的连接来管理事务范围
            MANAGED  这个配置几乎没做什么。它从来不提交或回滚一个连接，而会让容器来管理事务的整个生命周期（比如spring或JEE应用服务器的上下文）
             -->
            <transactionManager type="JDBC" />
            <!-- 
            UNPOOLED  这个数据源的实现是每次被请求时简单打开和关闭连接
            POOLED    mybatis实现的简单数据库连接池类型，它使得数据库连接可被复用，不必每次请求时都取创建一个物理连接
            JNDI      通过jndi从tomcat之类的容器里获取数据源
             -->
            <dataSource type="POOLED">
                <property name="driver" value="${driver}" />
                <property name="url" value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />
            </dataSource>
        </environment>
        
        <environment id="test">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/student" />
                <property name="username" value="root" />
                <property name="password" value="root" />
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <mapper resource="mapping/studentDao-mapping.xml" />
    </mappers>
</configuration>
```







#### 主要构件及其相互关系

从mybatis代码实现的角度来看，mybatis的核心部件有一下几个：

1. SqlSession作为mybatis工作的主要顶层API，表示和数据库交互的回话，完成必要数据库的增删改查功能
2. Executor作为mybatis的执行器，是mybatis调度的核心，负责SQL语句的生成和查询缓存的维护
3. statementHandler封装了JDBC statement操作，负责对JDBC statement的操作，如设置参数、将statement结果集转换成List集合
4. parameterHandler负责对用户传递的参数转换成JDBC Statement所需要的参数
5. ResultSetHandler复制将JDBC返回的ResultSet结果集对象转换成list类型的集合
6. TypeHandler负责Java数据类型和JDBC数据类型之间的映射和转换
7. MappedStatement 维护了一条<select|update|delete|insert>节点的封装
8. sqlsource负责根据用户传递的parameterObject，动态的生成SQL语句，将信息封装到BoundSql对象中，并返回
9. BoundSql表示动态生成的Sql语句已经相应的参数信息
10. Configuration：mybatis所有的配置信息都维持在Configuration对象之中。






#### mybatis XML配置文件的层次结构

这些层次结构是不能颠倒顺序的，否则，在解析XML文件的时候会出现异常

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>   <!--配置-->
	<properties/>	<!--属性-->
  	<setting/>	<!--设置-->
  	<typeAliases/>	<!--类型命名-->
  	<typeHandlers/>	<!--类型处理器-->
  	<objectFactiory/>	<!--对象工厂-->
  	<plugins/>	<!--插件-->
  	<environments>	<!--配置环境-->
    	<environment>	<!--环境变量-->
      		<transactionManager/>	<!--事务管理器-->
          	<datasource/>	<!--数据源-->
       	</environment>
    </environments>
  	<databaseIdProvider/>	<!--数据库厂商标识-->
  	<mappers/>	<!--映射器-->
</configuration>
```








