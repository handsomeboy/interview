# java有哪几种线程状态？如何转换？


Java线程的生命周期中，存在几种状态。在Thread类里有一个枚举类型State，定义了线程的几种状态:

![java线程状态](http://www.bcoder.top/img/interview/2.png)


下面来解释一下：

`NEW`: 线程创建之后，但是还没有启动(not yet started)。这时候它的状态就是NEW。

`RUNNABLE`: 正在Java虚拟机下跑任务的线程的状态。在RUNNABLE状态下的线程可能会处于等待状态， 因为它正在等待一些系统资源的释放，比如IO。

`BLOCKED`: 阻塞状态，等待锁的释放，比如线程A进入了一个synchronized方法，线程B也想进入这个方法，但是这个方法的锁已经被线程A获取了，这个时候线程B就处于BLOCKED状态

`WAITING`: 等待状态，处于等待状态的线程是由于执行了3个方法中的任意方法。
1.Object的wait方法，并且没有使用timeout参数; 
2.Thread的join方法，没有使用timeout参数 
3.LockSupport的park方法。 处于waiting状态的线程会等待另外一个线程处理特殊的行为。 再举个例子，如果一个线程调用了一个对象的wait方法，那么这个线程就会处于waiting状态直到另外一个线程调用这个对象的notify或者notifyAll方法后才会解除这个状态

`TIMED_WAITING`: 有等待时间的等待状态，比如调用了以下几个方法中的任意方法，并且指定了等待时间，线程就会处于这个状态。 
1. Thread.sleep方法 
2. Object的wait方法，带有时间 
3. Thread.join方法，带有时间 
4. LockSupport的parkNanos方法，带有时间 
5. LockSupport的parkUntil方法，带有时间

`TERMINATED`: 线程中止的状态，这个线程已经完整地执行了它的任务


下面是一张线程状态转化图：

![java线程状态](http://www.bcoder.top/img/interview/3.jpg)

线程间的状态转换（和Thread的state类似，但是有点区别）： 

1. 新建(new)：新创建了一个线程对象。

2. 可运行(runnable)：线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取cpu 的使用权 。

3. 运行(running)：可运行状态(runnable)的线程获得了cpu 时间片（timeslice） ，执行程序代码。

4. 阻塞(block)：阻塞状态是指线程因为某种原因放弃了cpu 使用权，也即让出了cpu timeslice，暂时停止运行。直到线程进入可运行(runnable)状态，才有机会再次获得cpu timeslice 转到运行(running)状态。阻塞的情况分三种： 

>(一). 等待阻塞：运行(running)的线程执行o.wait()方法，JVM会把该线程放入等待队列(waitting queue)中。
(二). 同步阻塞：运行(running)的线程在获取对象的同步锁时，若该同步锁被别的线程占用，则JVM会把该线程放入锁池(lock pool)中。
(三). 其他阻塞：运行(running)的线程执行Thread.sleep(long ms)或t.join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入可运行(runnable)状态。

5. 死亡(dead)：线程run()、main() 方法执行结束，或者因异常退出了run()方法，则该线程结束生命周期。死亡的线程不可再次复生。




