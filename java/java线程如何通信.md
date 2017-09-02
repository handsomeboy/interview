# java线程如何通信

## 同步
这里讲的同步是指多个线程通过synchronized关键字这种方式来实现线程间的通信。

```java
public class MyObject {
 
 synchronized public void methodA() {
  //do something....
 }
 
 synchronized public void methodB() {
  //do some other thing
 }
}
 
public class ThreadA extends Thread {
 
 private MyObject object;
//省略构造方法
 @Override
 public void run() {
  super.run();
  object.methodA();
 }
}
 
public class ThreadB extends Thread {
 
 private MyObject object;
//省略构造方法
 @Override
 public void run() {
  super.run();
  object.methodB();
 }
}
 
public class Run {
 public static void main(String[] args) {
  MyObject object = new MyObject();
 
  //线程A与线程B 持有的是同一个对象:object
  ThreadA a = new ThreadA(object);
  ThreadB b = new ThreadB(object);
  a.start();
  b.start();
 }
}
```
由于线程A和线程B持有同一个MyObject类的对象object，尽管这两个线程需要调用不同的方法，但是它们是同步执行的，比如：线程B需要等待线程A执行完了methodA()方法之后，它才能执行methodB()方法。这样，线程A和线程B就实现了通信。
这种方式，本质上就是`“共享内存”`式的通信。多个线程需要访问同一个共享变量，谁拿到了锁（获得了访问权限），谁就可以执行。

<br><br>

## while轮询的方式
代码如下：

```java
import java.util.ArrayList;
import java.util.List;
 
public class MyList {
 
 private List<String> list = new ArrayList<String>();
 public void add() {
  list.add("elements");
 }
 public int size() {
  return list.size();
 }
}
 
import mylist.MyList;
 
public class ThreadA extends Thread {
 
 private MyList list;
 
 public ThreadA(MyList list) {
  super();
  this.list = list;
 }
 
 @Override
 public void run() {
  try {
   for (int i = 0; i < 10; i++) {
    list.add();
    System.out.println("添加了" + (i + 1) + "个元素");
    Thread.sleep(1000);
   }
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
 }
}
 
import mylist.MyList;
 
public class ThreadB extends Thread {
 
 private MyList list;
 
 public ThreadB(MyList list) {
  super();
  this.list = list;
 }
 
 @Override
 public void run() {
  try {
   while (true) {
    if (list.size() == 5) {
     System.out.println("==5, 线程b准备退出了");
     throw new InterruptedException();
    }
   }
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
 }
}
 
import mylist.MyList;
import extthread.ThreadA;
import extthread.ThreadB;
 
public class Test {
 
 public static void main(String[] args) {
  MyList service = new MyList();
 
  ThreadA a = new ThreadA(service);
  a.setName("A");
  a.start();
 
  ThreadB b = new ThreadB(service);
  b.setName("B");
  b.start();
 }
}
```
在这种方式下，线程A不断地改变条件，线程ThreadB不停地通过while语句检测这个条件(list.size()==5)是否成立 ，从而实现了线程间的通信。`但是这种方式会浪费CPU资源`。之所以说它浪费资源，是因为JVM调度器将CPU交给线程B执行时，它没做啥“有用”的工作，只是在不断地测试 某个条件是否成立。就类似于现实生活中，某个人一直看着手机屏幕是否有电话来了，而不是： 在干别的事情，当有电话来时，响铃通知TA电话来了。关于线程的轮询的影响，

<br><br>
## wait/notify机制
代码如下：
```java
import java.util.ArrayList;
import java.util.List;
 
public class MyList {
 
 private static List<String> list = new ArrayList<String>();
 
 public static void add() {
  list.add("anyString");
 }
 
 public static int size() {
  return list.size();
 }
}
 
 
public class ThreadA extends Thread {
 
 private Object lock;
 
 public ThreadA(Object lock) {
  super();
  this.lock = lock;
 }
 
 @Override
 public void run() {
  try {
   synchronized (lock) {
    if (MyList.size() != 5) {
     System.out.println("wait begin "
       + System.currentTimeMillis());
     lock.wait();
     System.out.println("wait end "
       + System.currentTimeMillis());
    }
   }
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
 }
}
 
 
public class ThreadB extends Thread {
 private Object lock;
 
 public ThreadB(Object lock) {
  super();
  this.lock = lock;
 }
 
 @Override
 public void run() {
  try {
   synchronized (lock) {
    for (int i = 0; i < 10; i++) {
     MyList.add();
     if (MyList.size() == 5) {
      lock.notify();
      System.out.println("已经发出了通知");
     }
     System.out.println("添加了" + (i + 1) + "个元素!");
     Thread.sleep(1000);
    }
   }
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
 }
}
 
public class Run {
 
 public static void main(String[] args) {
 
  try {
   Object lock = new Object();
 
   ThreadA a = new ThreadA(lock);
   a.start();
 
   Thread.sleep(50);
 
   ThreadB b = new ThreadB(lock);
   b.start();
  } catch (InterruptedException e) {
   e.printStackTrace();
  }
 }
}
```


线程A要等待某个条件满足时(list.size()==5)，才执行操作。线程B则向list中添加元素，改变list 的size。

A,B之间如何通信的呢？也就是说，线程A如何知道 list.size() 已经为5了呢？

这里用到了Object类的 wait() 和 notify() 方法。
当条件未满足时(list.size() !=5)，线程A调用wait() 放弃CPU，并进入阻塞状态。---不像while轮询那样占用CPU
当条件满足时，线程B调用 notify()通知 线程A，所谓通知线程A，就是唤醒线程A，并让它进入可运行状态。
这种方式的一个好处就是CPU的利用率提高了。

但是也有一些**缺点**：比如，线程B先执行，一下子添加了5个元素并调用了notify()发送了通知，而此时线程A还执行；当线程A执行并调用wait()时，那它永远就不可能被唤醒了。因为，线程B已经发了通知了，以后不再发通知了。这说明：通知过早，会打乱程序的执行逻辑。

<br><br>

## Condition实现线程通信
Condition 接口描述了可能会与锁有关联的条件变量。这些变量在用法上与使用 Object.wait 访问的隐式监视器类似，但提供了更强大的功能。需要特别指出的是，单个 Lock 可能与多个 Condition 对象关联。为了避免兼容性问题，Condition 方法的名称与对应的 Object 版本中的不同。

在 Condition 对象中，与 wait、notify 和notifyAll 方法对应的分别是await、signal和signalAll。

Condition 实例实质上被绑定到一个锁上。要为特定 Lock 实例获得Condition 实例，请使用其 newCondition() 方法。

下面来回顾一下以前多线程下的生产者和消费者模式的demo，下面给出一种用wait和notifyAll的解决方案

```java
package demo1;
/*
 * 生产者和消费者案例
 */
public class TestProductorAndConsumer {
	public static void main(String[] args) {
		Clerk clerk = new Clerk();
		
		Productor pro = new Productor(clerk);
		Consumer cus = new Consumer(clerk);
		
		new Thread(pro, "生产者 A").start();
		new Thread(cus, "消费者 B").start();
		
		new Thread(pro, "生产者 C").start();
		new Thread(cus, "消费者 D").start();
	}
	
}
//店员
class Clerk{
	private int product = 0;
	
	//进货
	public synchronized void get(){//循环次数：0
		while(product >= 1){//为了避免虚假唤醒问题，应该总是使用在循环中
			System.out.println("产品已满！");
			
			try {
				this.wait();
			} catch (InterruptedException e) {
			}
			
		}
		
		System.out.println(Thread.currentThread().getName() + " : " + ++product);
		this.notifyAll();
	}
	
	//卖货
	public synchronized void sale(){//product = 0; 循环次数：0
		while(product <= 0){
			System.out.println("缺货！");
			
			try {
				this.wait();
			} catch (InterruptedException e) {
			}
		}
		
		System.out.println(Thread.currentThread().getName() + " : " + --product);
		this.notifyAll();
	}
}
//生产者
class Productor implements Runnable{
	private Clerk clerk;
	public Productor(Clerk clerk) {
		this.clerk = clerk;
	}
	@Override
	public void run() {
		for (int i = 0; i < 20; i++) {
			try {
				Thread.sleep(200);
			} catch (InterruptedException e) {
			}
			
			clerk.get();
		}
	}
}
//消费者
class Consumer implements Runnable{
	private Clerk clerk;
	public Consumer(Clerk clerk) {
		this.clerk = clerk;
	}
	@Override
	public void run() {
		for (int i = 0; i < 20; i++) {
			clerk.sale();
		}
	}
}

```
下面使用Lock和Condition来实现线程唤醒机制:
```java
package demo1;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
/*
 * 生产者和消费者案例
 */
public class TestProductorAndConsumerForLock {
	public static void main(String[] args) {
		Clerk clerk = new Clerk();
		
		Productor pro = new Productor(clerk);
		Consumer cus = new Consumer(clerk);
		
		new Thread(pro, "生产者 A").start();
		new Thread(cus, "消费者 B").start();
		
		new Thread(pro, "生产者 C").start();
		new Thread(cus, "消费者 D").start();
	}
	
}
//店员
class Clerk{
	private int product = 0;
	private Lock lock = new ReentrantLock();
	private Condition condition = lock.newCondition();
	
	//进货
	public void get(){
		
		lock.lock();
		try {
			while(product >= 1){//为了避免虚假唤醒问题，应该总是使用在循环中
				System.out.println("产品已满！");
				
				try {
					condition.await();
				} catch (InterruptedException e) {
				}
				
			}
			
			System.out.println(Thread.currentThread().getName() + " : " + ++product);
			condition.signalAll();
		} finally {
			lock.unlock();
		}
	}
	
	//卖货
	public  void sale(){
		lock.lock();
		try {
			while(product <= 0){
				System.out.println("缺货！");
				
				try {
					condition.await();
				} catch (InterruptedException e) {
				}
			}
			
			System.out.println(Thread.currentThread().getName() + " : " + --product);
			condition.signalAll();
		} finally {
			lock.unlock();
		}
	}
}
//生产者
class Productor implements Runnable{
	private Clerk clerk;
	public Productor(Clerk clerk) {
		this.clerk = clerk;
	}
	@Override
	public void run() {
		for (int i = 0; i < 20; i++) {
			try {
				Thread.sleep(200);
			} catch (InterruptedException e) {
			}
			
			clerk.get();
		}
	}
}
//消费者
class Consumer implements Runnable{
	private Clerk clerk;
	public Consumer(Clerk clerk) {
		this.clerk = clerk;
	}
	@Override
	public void run() {
		for (int i = 0; i < 20; i++) {
			clerk.sale();
		}
	}
}
```

下面拓展一下Condition
编写一个程序，开启 3 个线程，这三个线程的 ID 分别为A、B、C，每个线程将自己的 ID 在屏幕上打印 5 遍，要求输出的结果必须按顺序显示。
如：
ABC
ABC
ABC
```java
package demo1;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
public class LoopPrint {
	public static void main(String[] args) {
		TestLoop testLoop = new TestLoop();
		new Thread(new Runnable() {
			public void run() {
				for(int i=0; i<5;i++){
					testLoop.loopA();
				}
			}
		}).start();
		new Thread(new Runnable() {
			public void run() {
				for(int i=0; i<5;i++){
					testLoop.loopB();
				}
			}
		}).start();
		new Thread(new Runnable() {
			public void run() {
				for(int i=0; i<5;i++){
					testLoop.loopC();
				}
			}
		}).start();
	}
}
class TestLoop {
	private int flag = 1;
	private Lock lock = new ReentrantLock();
	private Condition condition1 = lock.newCondition();
	private Condition condition2 = lock.newCondition();
	private Condition condition3 = lock.newCondition();
	public void loopA() {
		lock.lock();
		try {
			if (flag != 1) {
				try {
					condition1.await();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			System.out.print("A");
			flag=2;
			condition2.signal();
		} finally {
			lock.unlock();
		}
	}
	public void loopB() {
		lock.lock();
		try {
			if (flag != 2) {
				try {
					condition2.await();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			System.out.print("B");
			flag=3;
			condition3.signal();
		} finally {
			lock.unlock();
		}
	}
	public void loopC() {
		lock.lock();
		try {
			if (flag != 3) {
				try {
					condition3.await();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			System.out.println("C");
			flag=1;
			condition1.signal();
		} finally {
			lock.unlock();
		}
	}
}
```



