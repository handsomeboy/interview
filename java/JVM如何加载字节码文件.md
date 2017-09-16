# JVM如何加载字节码文件?


JVM加载字节码文件（也就是.class文件，这是一个二进制文件）是通过类加载器，下面从头到尾理一下类加载器。
<br>

## 1. 什么是 Java 类加载机制?

Java 虚拟机一般使用 Java 类的流程为：首先将开发者编写的 `Java 源代码`（.java 文件）编译成 `Java 字节码`（.class 文件），然后`类加载器`会读取这个 .class 文件，并转换成 `java.lang.Class` 的实例。有了该 Class 实例后，Java 虚拟机可以利用 newInstance 之类的方`法创建其真正对象`了。

ClassLoader 是 Java 提供的类加载器，绝大多数的类加载器都继承自 ClassLoader，它们被用来加载不同来源的 Class 文件。

<br>

## 2. Class 文件有哪些来源呢?
上文提到了 ClassLoader 可以去加载多种来源的 Class，那么具体有哪些来源呢？

首先，最常见的是开发者在应用程序中编写的类，这些类位于项目目录下；

然后，有 Java 内部自带的 核心类 如 `java.lang`、`java.math`、`java.io` 等 package 内部的类，位于 `$JAVA_HOME/jre/lib/` 目录下，如 `java.lang.String` 类就是定义在 `$JAVA_HOME/jre/lib/rt.jar` 文件里；

另外，还有 Java 核心扩展类，位于 `$JAVA_HOME/jre/lib/ext` 目录下。开发者也可以把自己编写的类打包成 jar 文件放入该目录下；

最后还有一种，是`动态加载远程的 .class 文件`。

既然有这么多种类的来源，那么在 Java 里，是由某一个具体的 ClassLoader 来统一加载呢？还是由多个 ClassLoader 来协作加载呢？

<br>

## 3. 哪些 ClassLoader 负责加载上面几类 Class？
实际上，针对上面四种来源的类，分别有不同的加载器负责加载。

首先，我们来看级别最高的 **Java 核心类** ，即`$JAVA_HOME/jre/lib` 里的核心 jar 文件。这些类是 Java 运行的基础类，由一个名为 BootstrapClassLoader 加载器负责加载，它也被称作 **根加载器／引导加载器**。注意，BootstrapClassLoader 比较特殊，它不继承 ClassLoader，而是由 JVM 内部实现；

然后，需要加载 **Java 核心扩展类** ，即 `$JAVA_HOME/jre/lib/ext` 目录下的 jar 文件。这些文件由 ExtensionClassLoader 负责加载，它也被称作 **扩展类加载器**。当然，用户如果把自己开发的 jar 文件放在这个目录，也会被 **ExtClassLoader** 加载；

接下来是开发者在**项目中编写的类**，这些文件将由 AppClassLoader 加载器进行加载，它也被称作 **系统类加载器 System ClassLoader**；

最后，如果想远程加载如（本地文件／网络下载）的方式，则必须要自己自定义一个 ClassLoader，复写其中的 findClass() 方法才能得以实现。

因此能看出，Java 里提供了至少四类 ClassLoader 来分别加载不同来源的 Class。

那么，这几种 ClassLoader 是如何协作来加载一个类呢？

<br>

## 4. 这些 ClassLoader 以何种方式来协作加载 String 类呢？

String 类是 Java 自带的最常用的一个类，现在的问题是，JVM 将以何种方式把 String class 加载进来呢？

我们来猜想下。

首先，String 类属于 **Java 核心类**，位于 $JAVA_HOME/jre/lib 目录下。有的朋友会马上反应过来，上文中提过了，该目录下的类会由 BootstrapClassLoader 进行加载。没错，它确实是由 BootstrapClassLoader 进行加载。但，这种回答的前提是你已经知道了 String 在 $JAVA_HOME/jre/lib 目录下。

那么，如果你并不知道 String 类究竟位于哪呢？或者我希望你去加载一个 unknown 的类呢？

有的朋友这时会说，那很简单，只要去遍历一遍所有的类，看看这个 unknown 的类位于哪里，然后再用对应的加载器去加载。

是的，思路很正确。那应该如何去遍历呢？

比如，可以先遍历用户自己写的类，如果找到了就用 AppClassLoader 去加载；否则去遍历 Java 核心类目录，找到了就用 BootstrapClassLoader 去加载，否则就去遍历 Java 扩展类库，依次类推。

这种思路方向是正确的，不过存在一个漏洞。

假如开发者自己伪造了一个 java.lang.String 类，即在项目中创建一个包java.lang，包内创建一个名为 String 的类，这完全可以做到。那如果利用上面的遍历方法，是不是这个项目中用到的 String 不是都变成了这个伪造的 java.lang.String 类吗？如何解决这个问题呢？

解决方法很简单，当查找一个类时，优先遍历最高级别的 Java 核心类，然后再去遍历 Java 核心扩展类，最后再遍历用户自定义类，而且这个遍历过程是一旦找到就立即停止遍历。

在 Java 中，这种实现方式也称作 双亲委托。其实很简单，把 BootstrapClassLoader 想象为核心高层领导人， ExtClassLoader 想象为中层干部， AppClassLoader 想象为普通公务员。每次需要加载一个类，先获取一个系统加载器 AppClassLoader 的实例（ClassLoader.getSystemClassLoader()），然后向上级层层请求，由最上级优先去加载，如果上级觉得这些类不属于核心类，就可以下放到各子级负责人去自行加载。

如下图所示：

![JVM如何加载字节码文件?](http://www.bcoder.top/img/interview/29.jpg)

![JVM如何加载字节码文件?](http://www.bcoder.top/img/interview/32.jpg)

JVM是按照双亲委托的方式加载类，下面来看一下双亲委托机制

## 5. 从源码角度真正理解双亲委托加载机制


打开 ClassLoader 里的 loadClass() 方法，便是需要分析的源码了。这个方法里做了下面几件事：

检查目标 class 是否曾经加载过，如果加载过则直接返回；

如果没加载过，把加载请求传递给 parent 加载器去加载；

如果 parent 加载器加载成功，则直接返回；

如果 parent 未加载到，则自身调用 findClass() 方法进行寻找，并把寻找结果返回。

代码如下：

```java
protected Class<?> loadClass(String name, boolean resolve)
    throws ClassNotFoundException
{
    synchronized (getClassLoadingLock(name)) {
        // 1. 检查是否曾加载过
        Class<?> c = findLoadedClass(name);
        if (c == null) {
            long t0 = System.nanoTime();
            try {
                if (parent != null) {
                	// 优先让 parent 加载器去加载
                    c = parent.loadClass(name, false);
                } else {
                	// 如无 parent，表示当前是 BootstrapClassLoader，调用 native 方法去 JVM 加载
                    c = findBootstrapClassOrNull(name);
                }
            } catch (ClassNotFoundException e) {
                // ClassNotFoundException thrown if class not found
                // from the non-null parent class loader
            }
            if (c == null) {
            	// 如果 parent 均没有加载到目标 class，调用自身的 findClass() 方法去搜索
                long t1 = System.nanoTime();
                c = findClass(name);
                // this is the defining class loader; record the stats
                sun.misc.PerfCounter.getParentDelegationTime().addTime(t1 - t0);
                sun.misc.PerfCounter.getFindClassTime().addElapsedTimeFrom(t1);
                sun.misc.PerfCounter.getFindClasses().increment();
            }
        }
        if (resolve) {
            resolveClass(c);
        }
        return c;
    }
}
// BootstrapClassLoader 会调用 native 方法去 JVM 加载
private native Class<?> findBootstrapClass(String name);


```

下面抛开类加载器，讲讲类的加载

## 6. 类的加载过程  

JVM将类加载过程分为三个步骤：装载（Load），链接（Link）和初始化(Initialize)链接又分为三个步骤，如下图所示：

![JVM如何加载字节码文件?](http://www.bcoder.top/img/interview/30.png)

1)装载：查找并加载类的二进制数据；

2)链接：

验证：确保被加载类的正确性；

准备：为类的静态变量分配内存，并将其初始化为默认值； 

解析：把类中的符号引用转换为直接引用； 

3)初始化：为类的静态变量赋予正确的初始值；

那为什么要有验证这一步骤呢？首先如果由编译器生成的class文件，它肯定是符合JVM字节码格式的，但是万一有高手自己写一个class文件，让JVM加载并运行，用于恶意用途，就不妙了，因此这个class文件要先过验证这一关，不符合的话不会让它继续执行的，也是为了安全考虑吧。

准备阶段和初始化阶段看似有点牟盾，其实是不牟盾的，如果类中有语句：private static int a = 10，它的执行过程是这样的，首先字节码文件被加载到内存后，先进行链接的验证这一步骤，验证通过后准备阶段，给a分配内存，因为变量a是static的，所以此时a等于int类型的默认初始值0，即a=0,然后到解析，到初始化这一步骤时，才把a的真正的值10赋给a,此时a=10。

<br>

## 7.类的初始化

**类什么时候才被初始化：**

1）创建类的实例，也就是new一个对象 

2）访问某个类或接口的静态变量，或者对该静态变量赋值 

3）调用类的静态方法 

4）反射（Class.forName("com.zln.load")） 

5）初始化一个类的子类（会首先初始化子类的父类） 

6）JVM启动时标明的启动类，即文件名和类名相同的那个类
只有这6中情况才会导致类的类的初始化。

<br>

**类的初始化步骤：**

1）如果这个类还没有被加载和链接，那先进行加载和链接

2）假如这个类存在直接父类，并且这个类还没有被初始化（注意：在一个类加载器中，类只能初始化一次），那就初始化直接的父类（不适用于接口）

3)加入类中存在初始化语句（如static变量和static块），那就依次执行这些初始化语句。

<br>

## 8.类的加载

类的加载指的是将类的.class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区内，然后在堆区创建一个这个类的java.lang.Class对象，用来封装类在方法区类的对象。

![JVM如何加载字节码文件?](http://www.bcoder.top/img/interview/31.jpg)

![JVM如何加载字节码文件?](http://www.bcoder.top/img/interview/33.jpg)

类的加载的最终产品是位于堆区中的Class对象

Class对象封装了类在方法区内的数据结构，并且向Java程序员提供了访问方法区内的数据结构的接口

**加载类的方式有以下几种：**

1. 从本地系统直接加载
2. 通过网络下载.class文件
3. 从zip，jar等归档文件中加载.class文件
4. 从专有数据库中提取.class文件
5. 将Java源文件动态编译为.class文件（服务器）

