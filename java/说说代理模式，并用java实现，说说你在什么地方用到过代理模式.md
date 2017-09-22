# 说说代理模式，并用java实现，说说你在什么地方用到过代理模式


## 1. 什么叫代理？

代理是一种常用的设计模式，其目的就是为其他对象提供一个代理以控制对某个对象的访问。代理类**负责为委托类预处理消息**，过滤消息并转发消息，以及进行消息被委托类执行后的后续处理。

为了保持行为的一致性，代理类和委托类通常会实现相同的接口，所以在访问者看来两者没有丝毫的区别。通过代理类这中间一层，能有效控制对委托类对象的直接访问，也可以很好地隐藏和保护委托类对象，同时也为实施不同控制策略预留了空间，从而在设计上获得了更大的灵活性。

更通俗的说，代理解决的问题当两个类需要通信时，引入第三方代理类，将两个类的关系解耦，让我们只了解代理类即可，而且代理的出现还可以让我们完成与另一个类之间的关系的统一管理**，但是切记，代理类和委托类要实现相同的接口，因为代理真正调用的还是委托类的方法。**

## 2. 静态代理模式

### 2.1. 静态代理介绍

由程序员创建或工具生成代理类的源码，再编译代理类。所谓静态也就是在程序运行前就已经存在代理类的字节码文件，代理类和委托类的关系在运行前就确定了。

### 2.2. 静态代理代码实现

```java
package blog.reflect.proxy;


//静态代理模式
public class StaticProxy {
    public static void main(String[] args) {

        NickFactory niCKFactory = new NickFactory();
        LiningFactory liningFactory = new LiningFactory();



        ProxyFactory proxyFactory = new ProxyFactory(niCKFactory);

        proxyFactory.productCloth();

        proxyFactory = new ProxyFactory(liningFactory);
        proxyFactory.productCloth();

    }
}

interface ClothFactory{
    void productCloth();
}


class NickFactory implements ClothFactory{

    @Override
    public void productCloth() {
        System.out.println("Nick work");
    }
}


class LiningFactory implements ClothFactory{

    @Override
    public void productCloth() {
        System.out.println("Lining work");
    }
}


//代理类
class ProxyFactory implements ClothFactory{

    private ClothFactory cf;


    public ProxyFactory(ClothFactory cf) {
        this.cf = cf;
    }

    @Override
    public void productCloth() {
        System.out.println("生产即将开始");
        cf.productCloth();
    }
}
```

运行结果：

```
生产即将开始
Nick work
生产即将开始
Lining work
```

### 2.3. 静态代理有什么优缺点？


#### 2.3.1. 优点：
代理使客户端不需要知道实现类是什么，怎么做的，而客户端只需知道代理即可（解耦合）
 
#### 2.3.2. 缺点：

1）代理类和委托类实现了相同的接口，代理类通过委托类实现了相同的方法。这样就**出现了大量的代码重复**。如果接口增加一个方法，除了所有实现类需要实现这个方法外，所有代理类也需要实现此方法。增加了代码维护的复杂度。

2）代理对象只服务于一种类型的对象，如果要服务多类型的对象。势必要为每一种对象都进行代理，静态代理在程序规模稍大时就无法胜任了。


## 3. 动态代理模式

### 3.1. 动态代理介绍

根据如上的介绍，你会发现每个代理类只能为一个接口服务，这样程序开发中必然会产生许多的代理类
所以我们就会想办法可以通过一个代理类完成全部的代理功能，那么我们就需要用动态代理
 
在上面的示例中，一个代理只能代理一种类型，而且是在编译器就已经确定被代理的对象。而动态代理是在运行时，通过反射机制实现动态代理，并且能够代理各种类型的对象
 
在Java中要想实现动态代理机制，需要`java.lang.reflect.InvocationHandler`接口和 `java.lang.reflect.Proxy` 类的支持

### 3.2. 动态代理代码实现

```java
package blog.reflect.proxy;

import com.sun.xml.internal.bind.v2.ClassFactory;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;


//动态代理
public class DynamicProxy {


    public static void main(String[] args) {
        InvocationHandlerImpl invocationHandler = new InvocationHandlerImpl();

        NickFactory nickFactory = new NickFactory();
        ClothFactory blind = (ClothFactory) invocationHandler.blind(nickFactory);
        blind.productCloth();


        LiningFactory liningFactory = new LiningFactory();
        ClothFactory blind2 = (ClothFactory) invocationHandler.blind(liningFactory);
        blind2.productCloth();

    }

}


class InvocationHandlerImpl implements InvocationHandler {


    //所有类都继承Object，此处声明Object为了表示被代理类
    private Object object;

    public Object blind(Object object) {
        this.object = object;


        //该方法用于为指定类装载器、一组接口及调用处理器生成动态代理类实例
        //第一个参数指定产生代理对象的类加载器，需要将其指定为和目标对象同一个类加载器
        //第二个参数要实现和目标对象一样的接口，所以只需要拿到目标对象的实现接口
        //第三个参数表明这些被拦截的方法在被拦截时需要执行哪个InvocationHandler的invoke方法
        //根据传入的目标返回一个代理对象
        return Proxy.newProxyInstance(object.getClass().getClassLoader(),
                object.getClass().getInterfaces(), this);
    }


    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        //调用目标方法 ,此处也可以进行处理
        return method.invoke(object, args);
    }
}
```

运行结果：

```
Nick work
Lining work
```

### 3.3. 动态代理有什么优缺点？


#### 3.3.1. 优点：

动态代理与静态代理相比较，最大的好处是接口中声明的所有方法都被转移到调用处理器一个集中的方法中处理（InvocationHandler.invoke）。这样，在接口方法数量比较多的时候，我们可以进行灵活处理，而不需要像静态代理那样每一个方法进行中转。在本示例中看不出来，因为invoke方法体内嵌入了具体的外围业，实际中可以类似Spring AOP那样配置外围业务。 


#### 3.3.2. 缺点：

诚然，Proxy 已经设计得非常优美，但是还是有一点点小小的遗憾之处，那就是**它始终无法摆脱仅支持 interface 代理的桎梏**，因为它的设计注定了这个遗憾。回想一下那些动态生成的代理类的继承关系图，它们已经注定有一个共同的父类叫 Proxy。Java 的继承机制注定了这些动态代理类们无法实现对 class 的动态代理，原因是多继承在 Java 中本质上就行不通。 

## 4. 代理模式应用：AOP

###  4.1. AOP介绍

AOP（AspectOrientedProgramming）：将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码---解耦。

### 4.2. AOP代码实现

```java
package blog.reflect.proxy;

import org.jetbrains.annotations.NotNull;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class AOPTest {
    public static void main(String[] args) {

        Human superMan = (Human) MyProxy.getNewInstance(new SuperMan());
        superMan.info();
        System.out.println("=========================");
        superMan.fly();

    }
}

interface Human{
    void info();
    void fly();
}


class SuperMan implements Human{

    @Override
    public void info() {
        System.out.println("我是超人");
    }

    @Override
    public void fly() {
        System.out.println("我会飞");
    }
}

class HumanUtils{
    public void method1(){
        System.out.println("我是新加入的方法1");
    }

    public void method2(){
        System.out.println("我是新加入的方法2");
    }
}


class MyAop implements InvocationHandler{

    private Object object;

    public Object setObject(Object object){
        this.object = object;

        return Proxy.newProxyInstance(object.getClass().getClassLoader()
                ,object.getClass().getInterfaces(),this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        HumanUtils humanUtils = new HumanUtils();
        humanUtils.method1();
        Object invoke = method.invoke(object, args);
        humanUtils.method2();

        return invoke;
    }
}

class MyProxy{


    public static Object getNewInstance(Object obj){
        MyAop hander = new MyAop();
        hander.setObject(obj);
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(),
                obj.getClass().getInterfaces(), hander);
    }
}

```


运行结果：

```
我是新加入的方法1
我是超人
我是新加入的方法2
=========================
我是新加入的方法1
我会飞
我是新加入的方法2

```
