#### Spring框架的核心——IOC

##### 控制反转的思想：

Java中的业务逻辑都至少需要两个及以上的对象通过彼此的合作来实现的，这使得每个对象都需要与其合作的对象（也就是它所依赖的对象）的引用。通常都是通过new的方式获得它所依赖的对象，但是这样一来就会导致代码高度耦合和可测试性的降低。

IOC的思想就是：通过spring容器来实现这些相互依赖的对象的创建、协调工作，对象只需要关心业务逻辑本身就可以了。从这方面来说，对象如何得到它的协作对象的责任被反转了。



##### 总结：

应用控制反转后，当对象被创建时，由IOC容器内的所有对象的外界实体将其所依赖的对象的引用传递给它，即依赖被注入到对象中。所以，控制反转是关于一个对象如何获取它所依赖的对象的引用。这里的反转指的是责任的反转。（本来是自己new的，现在是通过IOC容器获取）



##### 关于IOC容器：

常见的BeanFactory和ApplicationContext都可以看做容器的具体表现形式。

我们通常说的IOC容器实际上代表着一系列功能各异的容器产品，只是容器的功能有大有小，有各自的特点。

以水桶为例，商店中的水桶有大有小，材料也各有不同，但只要能装水，具备水桶的基本特征，就可以作为水桶来出售。spring中也一样，有各式各样的IOC容器的实现供用户选择和使用。

在Spring中接口类BeanFactory体现了Spring为提供给用户使用的IOC容器所设定的最基本的功能规范。如果把IOC容器看做一个水桶，那么BeanFactory就定义了可以作为水桶的基本功能，如能装水。BeanFactory是作为一个最基本的接口类出现在Spring的IOC容器体系中的。

在这些Spring提供的基本IOC容器的接口定义和实现基础上，Spring通过定义BeanDefinition来管理基于Spring应用中的各种对象以及它们之间的相互依赖关系。BeanDefinition抽象了我们对Bean的定义，是让容器起作用的主要数据类型。











BeanFactory代码：

```
public interface BeanFactory {
    public static final String FACTORY_BEAN_PREFIX = "&";
    
    public abstract Object getBean(String paramString) throws BeansException;
    
    public abstract Object getBean(String paramString, Class ParamClass) throws BeansException;
    
    public abstract Object getBean(String paramString, Object[] paramArrayOfObject) throw BeansException;
    
    public abstract boolean containsBean(String paramString);
    
    public abstract boolean isSingleton(String paramString) throws NoSuchBeanDefinitionException;
    
    public abstract boolean isProtoType(String paramString) throws NoSuchBeanDefinitionException;
    
    public abstract boolean isTypeMatch(String paramString, Class paramClass) throws NoSuchBeanDefinitionException;
    
    public abstract String[] getAliases(String paramString);
}
```



##### 以XmlBeanFactory为例：









```
ClassPathResource res = new ClassPathResource("bean.xml");
DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(factory);
reader.loadBeanDefinitions(res);
```



##### 使用IOC容器的步骤：

1. 创建IOC配置文件的抽象资源，这个抽象资源包含了BeanDefinition的定义信息
2. 创建一个BeanFactory,这里使用的是DeafultListableBeanFactory
3. 创建一个载入BeanDefinition的读取器,这里使用的是XmlBeanDefinitionReader来载入xml文件形式的Beandefinition，通过一个回调配置给BeanFactory
4. 从定义好的资源的位置读入配置信息，具体的解析过程由XmlBeanDefinitionReader来完成

完成整个载入和注册Bean定义之后，需要的IOC容器就建立起来了。



