# **设计模式**

# 创建型模式   5种

## 单例模式

https://yq.aliyun.com/articles/11333

1. 懒汉式 synchronized
2. 双重检查
3. 静态内部类 比1,2要好

	```
	public class Singleton {

    private Singleton(){}
    /**
     *    类级的内部类，也就是静态的成员式内部类，该内部类的实例与外部类的实例
     *    没有绑定关系，而且只有被调用到时才会装载，从而实现了延迟加载。
     */
    private static class SingletonHolder{
        /**
         * 静态初始化器，由JVM来保证线程安全
         */
        private static Singleton instance = new Singleton();
    }

    public static Singleton getInstance(){
        return SingletonHolder.instance;
    }
}
	```

4. 饿汉式 静态私有变量

Java使用double check（双重检查）实现单例模式时，单例变量要使用volatile修饰 [原因](http://blog.csdn.net/u010660307/article/details/69922320)

## 工厂模式 (2种模式)

1. 简单工厂模式
2. 工厂方法模式 一个产品接口, 一个工厂接口,一个具体工厂对应一个具体产品
3. 抽象工厂模式 产品族 多个产品族接口, 多个工厂接口, 一个工厂接口生产一个产品族的多个产品

最终目的都是为了解耦。在使用时,我们具体问题具体分析

## 建造者模式

[Java设计模式（二）----建造者模式 by汤高](https://yq.aliyun.com/articles/11334)

- 抽象建造者（Builder）角色 ----> 接口,有一个具体的实现
- 导演者（Director）角色  ----> 聚合(contains)了 一个具体的Builder
- 产品（Product）角色 ----> 对象

建造模式分成两个很重要的部分

1. 一个部分是Builder接口，这里是定义了如何构建各个部件，也就是知道每个部件功能如何实现
2. 另外一个部分是Director，Director是知道如何组合来构建产品，也就是说Director负责整体的构建算法，而且通常是分步骤地来执行,也就是说如何组装这些部件。

## 原型模式 Prototype

重写 clone 方法 ,实现深复制

---------------------------------------

# 结构型模式

## 适配器模式

参考:[Java设计模式（六）----适配器模式 by汤高](https://yq.aliyun.com/articles/11337)



-----------------------------------------

### 模板方法模式

*定义一个操作中算法的框架，而将一些步骤延迟到子类中，使得子类可以不改变算法的结构即可重定义该算法中的某些特定步骤。*

**模板方法模式和策略模式有点像，而它其中又有些区别：**
> 1. 模板方法模式意图与策略模式意图不一样，模板方法模式工作是定义一个算法的大纲，而由其子类定义其中某些步骤的内容。

> 2. 模板方法模式对算法有更多的控制权，而且不会重复代码。不依赖任何对象，整个算法自己搞定。

参考：
> [设计模式之模板方法模式](https://zhuanlan.zhihu.com/p/25025808)


### 策略模式

*定义一系列的算法，把每一个算法封装起来，并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化。*

- 环境(Context)角色：持有一个Strategy的引用。
- 抽象策略(Strategy)角色：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。
- 具体策略(ConcreteStrategy)角色：包装了相关的算法或行为。


*参考：*
> [JAVA与模式之策略模式](http://www.cnblogs.com/java-my-life/archive/2012/05/10/2491891.html)


### 代理模式

#### 静态代理

*为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。*

proxy,它的目的在于保持接口不变，而改变接口定义的方法的行为。

在访问一个对象的时候进行log记录，检查参数等

对代理模式而言，一个主题类与一个代理r类一一对应，这是静态代理模式的特点。


#### 动态代理

多个主题类对应一个代理类，共享“前处理，后处理”功能，动态调用所需主题，大大减小了程序规模，这就是动态代理模式的特点。

动态代理类需要实现一个InvocationHandler接口

*InvocationHandler 接口*
> Object **invoke**(Object proxy, Method method, Object[] args) throws Throwable

> - **proxy**:   指代我们所代理的那个真实对象
- **method**: 指代的是我们所要调用真实对象的某个方法的Method对象
- **args**:   指代的是调用真实对象某个方法时接受的参数

> 每一个动态代理类都必须要实现InvocationHandler这个接口，并且每个代理类的实例都关联到了一个handler，当我们通过代理对象调用一个方法的时候，这个方法的调用就会被转发为由InvocationHandler这个接口的 invoke 方法来进行调用。

*Proxy 类*
> Proxy provides static methods for creating dynamic proxy classes and instances, and it is also the superclass of all dynamic proxy classes created by those methods.

**作用：**用来动态创建一个代理对象的类

*Proxy 的 newProxyInstance 方法*

> public static Object **newProxyInstance**(ClassLoader loader, Class<?>[] interfaces,  InvocationHandler h)  throws IllegalArgumentException

> 得到一个动态的代理对象，其接收三个参数

> - **loader:**代理对象的ClassLoader，定义了由哪个ClassLoader对象来对生成的代理对象进行加载,可以是业务接口的实现类，也可以是invocationHandler的实现类
- **interfaces:**代理对象的Interface的数组，表示的是我将要给我需要代理的对象提供一组什么接口，如果我提供了一组接口给它，那么这个代理对象就宣称实现了该接口(多态)，这样我就能调用这组接口中的方法了
- **h:**一个InvocationHandler对象，表示的是当我这个动态代理对象在调用方法的时候，会关联到哪一个InvocationHandler对象上

在程序执行中会生成这样一个代理类

*public final class $Proxy0 extends Proxy implements Subject*

通过 Proxy.newProxyInstance 创建的代理对象是在jvm运行时动态生成的一个对象，它并不是我们的InvocationHandler类型，也不是我们定义的那组接口的类型，而是在运行是动态生成的一个对象，并且命名方式都是这样的形式，以$开头，proxy为中，最后一个数字表示对象的标号。

在newProxyInstance这个方法的第二个参数上，我们给这个代理对象提供了一组什么接口，那么我这个代理对象就会实现了这组接口，这个时候我们当然可以将这个代理对象强制类型转化为这组接口中的任意一个。

通过这个代理对象，$proxy0实现的接口的方法会自动关联到实现了invocationHandler接口的类的invoke方法上


method.invoke(subject, args);//调用真实对象的方法

参考：
> *[代理模式](https://zhuanlan.zhihu.com/p/26141688)*

> *[动态代理](https://zhuanlan.zhihu.com/p/26193963)*

> *[java的动态代理机制](http://www.cnblogs.com/xiaoluo501395377/p/3383130.html)*


 CGLib 简介

> JDK的动态代理机制只能代理实现了接口的类，而不能实现接口的类就不能实现JDK的动态代理，cglib是针对类来实现代理的，他的原理是对指定的目标类生成一个子类，并覆盖其中方法实现增强，但因为采用的是继承，所以不能对final修饰的类进行代理。



### 观察者模式

定义：定义对象之间的一对多依赖关系，以便当一个对象更改状态时，将通知其所有依赖关系。

Observer模式当中不存在对应EventObject的角色，Observable被观察者就兼具了source事件源和EventObject事件对象两种角色，模型更简洁。

jdk：
> - 被观察者 继承 Observeable
- 观察者 实现 Observer update

### 门面模式

　外观模式（Facade）,他隐藏了系统的复杂性，并向客户端提供了一个可以访问系统的接口。这种类型的设计模式属于结构性模式。为子系统中的一组接口提供了一个统一的访问接口，这个接口使得子系统更容易被访问或者使用。

使用场景

1. 为复杂的模块或子系统提供外界访问的模块；
2. 子系统相互独立；
3. 在层析结构中，可以使用外观模式定义系统的每一层的入口。



### 装饰者模式

装饰器的好处不仅能够封装原有的功能，为原本已经存在的功能添加新的职责(如上述的添加计数等等)，而且还可以在已有功能的基础上添加新的功能。

**装饰者模式和代理模式**

装饰器模式应当为所装饰的对象提供增强功能，而代理模式对所代理对象的使用施加控制，并不提供对象本身的增强功能。

代理模式的特点在于隔离，隔离调用类和被调用类的关系，通过一个代理类去调用。

装饰类必须把被装饰的对象当作参数传入。这就是和代理模式的代码不同之处，代理模式一定是自身持有这个对象，不需要从外部传入。

而装饰模式的一定是从外部传入，并且可以没有顺序，按照代码的实际需求随意挑换顺序，就如你吃火锅先放白菜还是先放丸子都可以。





