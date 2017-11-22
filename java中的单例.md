### 1. 什么是单例

单例对象的类必须保证只有一个实例存在

对单例的实现可以分为两大类————懒汉式和饿汉式，它们的区别在于：
> 懒汉式： 指全局的单例实例在第一次使用时构建
  饿汉式： 指全局的单例实例在类装载时构建

日常使用较多的应该是懒汉式的单例，按需加载才能做到资源的最大化利用。

### 2. 懒汉式单例

#### 2.1 简单版本
首先是最简单的写法 version 1：

```
//version 1
public class Single1 {
    private static Single1 instance;
    public static Single1 getInstance() {
      if(instance == null) {
        instance = new Single1();
      }
      return instance;
    }
}
```
或者再进一步： 把构造器改为私有的，这样就能防止被外部的类调用
```
//version 1.1
public class Single1 {
  private static Single1 instance;
  private Single1() {}
  public static Single1 getInstance() {
    if (instance == null) {
      instance = new Single1();
    }
    return instance;
  }
}
```

每次获取instance之前先进行判断，如果instance为空，就new一个出来，否则就直接返回已经存在的instance

这种写法在大多数的时候是没有问题的。问题在于：**当多线程工作的时候，如果有多个线程同时运行到
if (instance == null)  都判断为null，那么两个线程就会各自创建一个实例，这样一来，就不是单例了。**

#### 2.2 synchronized版本

既然可能因为多线程导致问题，那就加上一个同步锁吧。

修改后的代码和version 1 相比只是在方法签名上多加了一个synchronized：

```
//version 2
public class Single2 {
  private static Single2 instance;
  private Single2() {}
  public static synchronied Single2 getInstance() {
    if (instance == null) {
      instance = new Single2()
    }
    return instance;
  }
}
```

当加上synchronized关键字后，getInstance方法就会锁上了。如果有两个线程(T1, T2)同时执行到这个方法时，会有其中一个线程T1获得同步锁，得以继续执行，而另一个线程T2则需要等待，当T1执行完毕getInstance之后（完成了null判断、对象创建、获得返回值之后），T2线程才会执行。这样就避免了version1中可能因为多线程导致出现多个实例的情况。

但是，这种写法也有一个问题：**给getInstance方法加锁，虽然会避免了可能会出现的多个实例问题，但是会强制要求除T1之外的所有线程等待，实际上会对程序的执行效率造成负面影响。**

#### 2.3 双重检查（Double-Check）版本
version 2 相对于version 1 代码的效率问题，其实是为了解决1%几率的问题而使用了一个100%出现的防护盾。那有一个优化的思路，就是把100%出现的防护盾也改为1%几率出现，使之只出现在可能会导致多个实例出现的地方。

```
//version 3
public class Single3 {
  private static Single3 instance;
  public Single3() {}
  public static Single3 getInstance() {
    if (instance == null) {
      synchronized (Single3.class) {
        if (instance == null) {
          instance = new Single3();
        }
    }
    return instance;
  }
}
```

version 3 看起来有点复杂，注意其中的两次 if (instance == null) 的判断，这个叫做**双重检查（Double-Check）**

- 第一个 if(instance == null) 是为了解决version 2中的效率问题，只有instance为null的时候，才进入synchronized的代码段，大大减少了几率
- 第二个 if(instance == null) 则和version 2 一样，是为了防止可能出现多个实例的情况

这段代码看起来完美无瑕了，其实还是有小概率出现问题的。
为了弄清楚为什么会出现问题，需要弄清楚几个概念：**原子操作、指令重排**

##### 原子操作
简单来说，原子操作就是不可分割的操作，在计算机中就是指不会因为线程调度而被打断的操作
比如简单的赋值语句就是一个原子操作
> m = 6; //这是一个原子操作

假如变量m原来的值是0，，那么对于这个操作，执行成功后，m的值变成了6，要么没执行，m还是0，而不会出现诸如m=3这种中间态。

而声明并赋值就不是一个原子操作：
> int n = 6;  //这不是一个原子操作

对于这个语句，至少有两个操作：
1. 声明一个变量n
2. 给n赋值为6

这样就会有一个中间态：变量n已经声明但是还没有赋值的状态。这样在多线程中，由于线程执行顺序的不确定性，如果两个线程都使用n，就可能会导致不稳定结果的出现。

##### 指令重排
简单来说，就是计算机为了提高执行效率，会做的一些优化，在不影响最终结果的情况下，可能会对一些语句的执行顺序进行调整。
比如以下代码：
```
int a;            //语句1
a = 8;            //语句2
int b = 9;        //语句3
int c = a + b;    //语句4
```

正常来说，对于顺序结构，执行的顺序是自上而下，即1234
但是由于指令重排的原因，因为不影响最终的结果，所以实际执行的顺序可能变成3124或者1324。由于语句3和语句4没有原子性的问题，语句3和语句4也可能会拆分成原子操作，再重排。也就是说，对于非原子性的操作，在不影响最终结果的情况下，其拆分成的原子操作可能会被重新排列执行顺序。

了解原子操作和指令重排之后，可以关注下version 3中的问题。

version 3 中的问题主要是在： instance = new Single3(); 这行代码中。因为这条语句并不是一个原子操作，事实上在JVM中这条语句大概做了下面3件事情：
1. 给Single3分配内存
2. 调用Single3的构造函数来初始化成员变量，形成实例
3. 将Single3对象指向分配的内存空间（执行完这步Single3才是非null的）

但是在JVM的即时编译器中存在指令重排序的优化。也就是说上面的第二步和第三步的顺序是不能保证的，最终的执行顺序可能是1-2-3也可能是1-3-2.如果是后者，则在3执行完毕、2未执行之前，被线程2抢占了，这时instance已经非null了（但却没有初始化），所以线程2会直接返回instance，然后使用，然后顺理成章的报错。


就是说，instance = new Single3() 有一个 **instance已经不为null但是仍没有初始化** 的中间态。而这个时候，如果有其他线程刚好运行到第一层的 if (instance == null) 这里，这里读取到的instance已经不是null了，所以就会直接把这个中间态的instance拿去用了，就会产生问题。这里的关键在于：**线程1对instance的写操作没有完成，线程2就执行了读操作**


#### 2.4 终极版本： volatile
对于version 3 中可能会出现的概率非常小的问题，解决方案是：给instance的声明加上关键字volatile即可

```
//version 4
public class Single4 {
  private static volatile Single4 instance;
  private Single4() {}
  public static Single4 getInstance() {
    if (instance == null) {
      synchronized (Single4.class) {
        if (instance == null) {
          instance = new Single4();
        }
      }
    }
    return instance;
  }
}
```

volatile 关键字的作用是禁止指令重排，把instance声明为volatile之后，对它的写操作就会有一个内存屏障，这样，在它的赋值完成之前就不会调用读操作。

注意： volatile阻止的不是instance = new Single3() 这条语句的内部1-2-3的指令重排，而是保证了在一个写操作1-2-3完成之前不会调用读操作（if (instance == null) ）

### 3. 饿汉式单例

饿汉式单例是指：全局的单例实例在类装载时构建的实现方式
由于类装载的过程是有类加载器（ClassLoader）来执行的，这个过程也是由JVM来保证同步的，所以这种方式先天就由一个优势：能够免疫许多由多线程引起的问题。

#### 3.1 饿汉式单例的实现方式
```
//饿汉式实现
public class Single {
  private static final Single INSTANCE = new Single();
  private Single() {}
  public staic Single getInstance() {
    return INSTANCE;
  }
}
```

饿汉式的缺点就是：INSTANCE的初始化是在类加载时进行的，而类的加载是由ClassLoader来做的，所以开发者本来对它的初始化的时机就很难去准确把握：
1. 可能由于初始化的太早，造成资源的浪费
2. 如果初始化本身依赖于一些其他数据，那么也就很难保证其他数据会在它初始化之前准备好。

当然，如果所需的单例占用的资源很少，并且也不依赖于其他数据，那么这种实现方式也是很好的。

前面提到了单例在类装载时被实例化，，那么什么时类装载时呢？

不严格的说，大致有这么几个条件会触发一个类被加载：
1. new一个对象时
2. 使用反射创建它的实例时
3. 子类被加载时，如果父类还没被加载，就先加载父类
4. JVM启动时执行的主类会首先被加载

### 4. 一些其他的实现方式

#### 4.1 Effective Java 1 ————静态内部类
```
//Effective Java 第一版推荐写法
public class Singleon {
  private static class SingletonHolder {
    private static final Singleton INSTANCE = new Singleton();
  }
  private Singleton() {}
  public static final Singleton getInstance() {
    return SingletonHolder.INSTANCE;
  }
}
```

这中写法非常巧妙：

- 对于内部类SingletonHolder，它是一个饿汉式的单例实现，在SingletonHolder初始化的时候会由ClassLoader来保证同步，是INSTANCE是一个真·单例
- 同时，由于SingletonHolder是一个内部类，只在外部类的Singleton的getInstance()中被使用，所以，它被加载的时机也就是在getInstance()方法第一次被调用的时候。

也就是说：**它利用了ClassLoader来保证了同步，同时又能让开发者控制类加载的时机，从内部看是一个饿汉式的单例，但是从外部看，又的确是懒汉式的实现。**

#### 4.2 Effective Java 2 ————枚举
```
//Effective Java 第二版推荐写法
public enum SingleInstance {
  INSTANCE;
  public void fun() {
    // do something...
  }
}

//使用
SingleInstance.INSTANCE.fun();
```

由于创建枚举实例的过程是线程安全的，所以这种写法也没有同步的问题。

对这种写法的评价：
> 这种写法在功能上与共有域方法相近，但是它更简洁，无偿的提供了序列化机制，绝对防止对此实例化，即使是在面对复杂的序列化或者反射攻击的时候。虽然这种方面还没有广泛采用，但是单元素的枚举类型已经成为实现Singleton的最佳方法。

不过，这种方法在需要继承的场景中就不适用了。
