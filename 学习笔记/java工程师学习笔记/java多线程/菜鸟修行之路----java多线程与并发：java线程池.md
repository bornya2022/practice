# 菜鸟修行之路----java多线程与并发：java线程池

​       线程池的实现过程没有用到Synchronized关键字，用的都是Volatile,Lock和同步(阻塞)队列,Atomic相关类，FutureTask等等，因为后者的性能更优

​      线程池的优点：

- 线程复用
- 控制最大并发数
- 管理线程

## 1.池化技术

 程序的运行本质上就是对系统资源（CPU、内存、磁盘、网络等等）的使用。线程池就是对于CPU利用的优化手段。

线程池的核心就是：**池化技术**

池化技术：提前保存大量的资源，以备不时之需。在机器资源有限的情况下，使用池化技术可以大大的提高资源的利用率，提升性能等。

池化技术的经典应用：

- 线程池
- 连接池
- 内存池
- 对象池

池化技术在java多线程中的具体思想：**通过预先创建好多个线程，放在池中，这样可以在需要使用线程的时候直接获取，避免多次重复创建、销毁带来的开销。**

**线程池的优点**

- 降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
- 提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。
- 提高线程的可管理性。线程是稀缺资源，如果无限制地创建，不仅会消耗系统资源,还会降低系统的稳定性，使用线程池可以进行统一分配、调优和监控。

## 2.线程池框架Executor

java中的线程池是通过Executor框架实现的。具体架构如下图所示：

![1584235887052](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1584235887052.png)



Executor 框架中主要类：

| 类                     | 功能                                                         |
| ---------------------- | ------------------------------------------------------------ |
| **Executor**           | 所有线程池的接口。                                           |
| **ExecutorService**    | 增加Executor的行为，是Executor实现类的最直接接口。           |
| **Executors**          | 提供了一系列工厂方法用于创建线程池，返回的线程池都实现了ExecutorService 接口。 |
| **ThreadPoolExecutor** | 线程池的具体实现类,常用线程池都是基于这个类实现的。          |

### 2.1 Executor

所有线程池的接口,只有一个方法。

```java
public interface Executor {

    /**
     * Executes the given command at some time in the future.  The command
     * may execute in a new thread, in a pooled thread, or in the calling
     * thread, at the discretion of the {@code Executor} implementation.
     *
     * @param command the runnable task
     * @throws RejectedExecutionException if this task cannot be
     * accepted for execution
     * @throws NullPointerException if command is null
     */
    void execute(Runnable command);
}
```

### 2.2 ThreadPoolExecutor

Executor框架最核心的类是ThreadPoolExecutor，这是各个线程池的实现类。

源码如下：

```java
public class ThreadPoolExecutor extends AbstractExecutorService {
    ...
    /*线程池的核心线程数,线程池中运行的线程数也永远不会超过 corePoolSize 个,默认情况下可以一直存活。可以通过设置allowCoreThreadTimeOut为True,此时 核心线程数就是0,此时keepAliveTime控制所有线程的超时时间。*/
    private volatile int corePoolSize;
    //线程池允许的最大线程池数
    private volatile int maximumPoolSize;
    //休眠等待时间，也称为空闲线程结束的超时时间
    private volatile long keepAliveTime;
    ...
    //构造函数
    /**
    *unit ：是一个枚举，表示 keepAliveTime 的单位;
    *workQueue：表示存放任务的BlockingQueue<Runnable>队列。
    *ThreadFactory： 创建线程的工厂
    */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             threadFactory, defaultHandler);
    }
}
```

**BlockingQueue（阻塞队列）**

​        阻塞队列（BlockingQueue）是java.util.concurrent下的主要用来控制**线程同步**的工具。如果BlockQueue是空的,从BlockingQueue取东西的操作将会被阻断进入等待状态,直到BlockingQueue进了东西才会被唤醒。同样,如果BlockingQueue是满的,任何试图往里存东西的操作也会被阻断进入等待状态,直到BlockingQueue里有空间才会被唤醒继续操作。
​        阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。阻塞队列就是生产者存放元素的容器，而消费者也只从容器里拿元素。具体的实现类有LinkedBlockingQueue,ArrayBlockingQueued等。一般其内部的都是通过Lock和Condition来实现阻塞和唤醒。

## 3 .线程池工作流程

- 线程池刚创建时，里面没有一个线程。任务队列是作为参数传进来的。

- 当调用 execute() 方法添加一个任务时，线程池会做如下判断：

- - 如果正在运行的线程数量小于 corePoolSize，那么马上创建线程运行这个任务；
  - 如果正在运行的线程数量大于或等于 corePoolSize，那么将这个任务放入队列；
  - 如果这时候队列满了，而且正在运行的线程数量小于 maximumPoolSize，那么还是要创建非核心线程立刻运行这个任务；
  - 如果队列满了，而且正在运行的线程数量大于或等于 maximumPoolSize，那么线程池会抛出异常RejectExecutionException。

- 当一个线程完成任务时，它会从队列中取下一个任务来执行。

- 当一个线程无事可做，超过一定的时间（keepAliveTime）时，线程池会判断，如果当前运行的线程数大于 corePoolSize，那么这个线程就被停掉。所以线程池的所有任务完成后，它最终会收缩到 corePoolSize 的大小。

## 4.线程池的创建以及使用

### 4.1创建线程池

生成线程池采用了工具类Executors的静态方法。

Executors提供了一系列工厂方法用于创建线程池，返回的线程池都实现了ExecutorService 接口。

通过Executor框架的根据类Executors，可以创建4种基本的线程池：

- FixedThreadPool 
- SingleThreadExecutor 
- CachedThreadPool 
- ScheduledThreadPool

**FixedThreadPool**  可重用固定线程数的线程池

​        FixedTheadPool设置的线程池大小和最大数量一样；keepAliveTime为0，代表多余的空闲线程会立刻终止；保存任务的队列使用LinkedBlockingQueue，当线程池中的线程执行完任务后，会循环反复从队列中获取任务来执行。
​      FixedThreadPool适用于限制当前线程数量的应用场景，适用于**负载比较重**的服务器。

```java
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
            0L, TimeUnit.MILLISECONDS,
            new LinkedBlockingQueue<Runnable>());
}
```

**SingleThreadExecutor** 单个后台线程 (其缓冲队列是无界的)

​       SingleThreadExecutor的核心线程池数量corePoolSize和最大数量maximumPoolSize都设置为1，适用于需要**保证顺序执行**的场景

```java
public static ExecutorService newSingleThreadExecutor() {
    return new FinalizableDelegatedExecutorService (
        new ThreadPoolExecutor(1, 1,
        0L, TimeUnit.MILLISECONDS,
        new LinkedBlockingQueue<Runnable>()));
}
```

**CachedThreadPool ** 无界线程池，可以进行自动线程回收

​    CachedThreadPool是一个会根据需要创建新线程的线程池，适用于短期异步的小任务，或**负载较轻**的服务器。

​      如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。   

```java
public static ExecutorService newCachedThreadPool() {
    return new ThreadPoolExecutor(0,Integer.MAX_VALUE,
           60L, TimeUnit.SECONDS,
           new SynchronousQueue<Runnable>());
}
```

SynchronousQueue是一个是缓冲区为1的阻塞队列。

**ScheduledThreadPool**   核心线程池固定，大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。

​     创建一个周期性执行任务的线程池。如果闲置,非核心线程池会在DEFAULT_KEEPALIVEMILLIS时间内回收。

```java
public static ExecutorService newScheduledThreadPool(int corePoolSize) {
    return new ScheduledThreadPool(corePoolSize,
              Integer.MAX_VALUE,
              DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,
              new DelayedWorkQueue());
}
```

### 4.2 线程池的应用

使用Executor框架创建线程池并且使用

代码实例：

```java
public class JavaThreadPool {
    public static void main(String[] args) {
        // 创建一个可重用固定线程数的线程池（最大线程数为2）
        ExecutorService pool = Executors.newFixedThreadPool(2);
        // 创建实现了Runnable接口对象，Thread对象当然也实现了Runnable接口
        Thread t1 = new MyThread();
        Thread t2 = new MyThread();
        Thread t3 = new MyThread();
        Thread t4 = new MyThread();
        Thread t5 = new MyThread();
        // 将线程放入池中进行执行
        pool.execute(t1);
        pool.execute(t2);
        pool.execute(t3);
        pool.execute(t4);
        pool.execute(t5);
        // 关闭线程池
        pool.shutdown();
    }
}
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "正在执行… …");
    }

}
```



修行之路艰辛，与君共勉

​                                                                                                                                          ----2020年3年 成都