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













