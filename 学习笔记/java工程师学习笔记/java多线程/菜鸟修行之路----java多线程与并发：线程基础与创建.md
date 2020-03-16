# 菜鸟修行之路----java多线程与并发：线程基础与创建



**多线程与并发核心知识点**：

![1583975312275](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583975312275.png)

## 1.线程基础

### 1.1线程与进程

**进程：**是计算机内存中运行的应用程序，有自己独立的地址空间，并且不同的进程的地址空间是相互隔离的，是**资源分配的最小单位。**

**线程：**是程序执行的最小单位，它是进程中的一个实体，线程本身是不会独立存在的。一个进程至少有一个线程，进程中的多个线程是共享进程资源的。每个线程有自己的堆栈和局部变量，它是**CPU调度和分配的基本单位**。

二者区别：

- 进程是资源分配的最小单位，线程是程序执行的最小单位；
- 同一个进程中可以包括多个线程，并且线程共享整个进程的资源（寄存器、堆、上下文），一个进程至少包括一个线程。
- 进程拥有自己独立的地址空间，包含程序内容和数据，并且不同进程的地址空间都是相互隔离的。而同一进程中的线程共用相同的地址空间，同时共享进程所拥有的内存和其他资源。
- 进程之间的切换开销大，而线程切换开销比较小。

### 1.2 单线程与多线程

**单线程**：程序中只存在一个线程，实际上通常main主方法就是一个主线程。

**多线程**：通常为了更好地使用CPU资源，在一个程序中运行了多个任务（也就是指这个程序或进程在运行时产生了不止一个线程）

使用多线程有两个目的：

1）更好的利用cpu的资源

单核CPU上所谓的“多线程”那是假的多线程，同一时间处理器只会处理一段逻辑，只不过线程之间切换得比较快，看着像多个线程“同时”运行罢了。

2）模拟“多角色”的实际场景，每个角色分配一个线程，比如常见的“生产者，消费者模型”

### 1.3 并行和并发

**并行：**parallel programming，多个cpu实例或者多台机器同时执行一段处理逻辑，是真正的同时。

**并发：**concurrent programming，通过cpu调度算法，让用户看上去同时执行，实际上从cpu操作层面不是真正的同时。并发往往在场景中有公用的资源，那么针对这个公用的资源往往产生瓶颈，我们会用TPS或者QPS来反应这个系统的处理能力。

注：

**QPS**：Queries Per Second意思是“每秒查询率”，是一台服务器每秒能够相应的查询次数，是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。

**TPS：**是TransactionsPerSecond的缩写，也就是事务数/秒。它是软件测试结果的测量单位。一个事务是指一个客户机向服务器发送请求然后服务器做出反应的过程。客户机在发送请求时开始计时，收到服务器响应后结束计时，以此来计算使用的时间和完成的事务个数，

经典的并行与并发图解：

![1583975709444](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583975709444.png)

### 1.4 线程安全与同步

**线程安全：**当多个线程访问某个方法时，不管你通过怎样的调用方式或者说这些线程如何交替的执行，我们在主程序中不需要去做任何的同步，这个类的结果行为都是我们设想的正确行为，那么我们就可以说这个类是线程安全的。

**同步：**Java中的同步指的是通过人为的控制和调度，保证共享资源的多线程访问成为线程安全，来保证结果的准确。如上面的代码简单加入@synchronized关键字。在保证结果准确的同时，提高性能，才是优秀的程序。线程安全的优先级高于性能。

## 2.线程状态

### 2.1线程状态

- **新建状态（New）：**线程对象被创建后，就进入了新建状态，如：Thread t = new MyThread();

- **就绪状态（Runnable）：**当调用线程对象的start()方法（t.start();），线程即进入就绪状态。处于就绪状态的线程，只是说明此线程已经做好了准备，随时等待CPU调度执行，并不是说执行了t.start()此线程立即就会执行；

- **运行状态（Running）：**当CPU开始调度处于就绪状态的线程时，此时线程才得以真正执行，即进入到运行状态。注：就绪状态是进入到运行状态的唯一入口，也就是说，线程要想进入运行状态执行，首先必须处于就绪状态中；

- **阻塞状态（Blocked）：**处于运行状态中的线程由于某种原因，暂时放弃对CPU的使用权，停止执行，此时进入阻塞状态，直到其进入到就绪状态，才有机会再次被CPU调用以进入到运行状态。根据阻塞产生的原因不同，阻塞状态又可以分为三种：

  1）等待阻塞：运行状态中的线程执行wait()方法，使本线程进入到等待阻塞状态，直到notify()/notifyAll()，线程被唤醒，获取同步锁后，线程回到可运行状态；

  2）同步阻塞：线程在获取synchronized同步锁失败(因为锁被其它线程所占用)，它会进入同步阻塞状态；

  3）其他阻塞：通过调用线程的sleep()或join()或发出了I/O请求时，线程会进入到阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。

- **死亡状态（Dead）：**线程执行完了或者因异常退出了run()方法，该线程结束生命周期。

### 2.2**线程状态转换图**

![1583976153417](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583976153417.png)

**就绪状态转换为运行状态：**当此线程得到处理器资源；

**运行状态转换为就绪状态：**当此线程主动调用yield()方法或在运行过程中失去处理器资源。

**运行状态转换为死亡状态：**当此线程线程执行体执行完毕或发生了异常。

**特别注意：**当调用线程的yield()方法时，线程从运行状态转换为就绪状态，但接下来CPU调度就绪状态中的哪个线程具有一定的随机性，因此，可能会出现A线程调用了yield()方法后，接下来CPU仍然调度了A线程的情况。

## 3.线程的优先级

每一个 Java 线程都有一个优先级，这样有助于操作系统确定线程的调度顺序。

Java 线程的优先级是一个整数，其取值范围是 1 （Thread.MIN_PRIORITY ） - 10 （Thread.MAX_PRIORITY ）。

默认情况下，每一个线程都会分配一个优先级 NORM_PRIORITY（5）。

具有较高优先级的线程对程序更重要，并且应该在低优先级的线程之前分配处理器资源。但是，线程优先级不能保证线程执行的顺序，而且非常依赖于平台。

## 4.线程创建

在java中，一般线程的创建有以下3种方式：

- **继承Thread()类，重写该类的run()方法**
- **实现Runnable接口，重写该类的run方法**
- **使用Callable和Future接口创建线程**

**线程创建与使用常用方法总结（Thread类）：**

| 方法                          | 功能                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| **start()**                   | 使该线程开始执行；**Java** 虚拟机调用该线程的 run 方法。     |
| **run()**                     | 如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，该方法不执行任何操作并返回。 |
| **setName(String name)**      | 设置线程名称                                                 |
| **setPriority(int priority)** | 更改线程的优先级。                                           |
| **setDaemon(boolean on)**     | 将该线程标记为守护线程或用户线程。                           |
| **join(long millisec)**       | 等待该线程终止的时间最长为 millis 毫秒。                     |
| **interrupt()**               | 中断线程。                                                   |
| **isAlive()**                 | 测试线程是否处于活动状态。                                   |
| **yield()**                   | 暂停当前正在执行的线程对象，并执行其他线程。**（线程礼让）** |
| **sleep(long millisec)**      | 在指定的毫秒数内让当前正在执行的线程休眠（暂停执行），此操作受到系统计时器和调度程序精度和准确性的影响。 |
| **holdsLock(Object x)**       | 当且仅当当前线程在指定的对象上保持监视器锁时，才返回 true。  |
| **currentThread()**           | 返回对当前正在执行的线程对象的引用。                         |
| **dumpStack()**               | 将当前线程的堆栈跟踪打印至标准错误流。                       |



### 3.1 继承Thread()类，重写该类的run()方法

Thread 类本质上是实现了 Runnable 接口的一个实例，代表一个线程的实例。**启动线程的唯一方法就是通过 Thread 类的 start()实例方法。**start()方法是一个 native 方法，它将启动一个新线
程，并执行 run()方法。

```java
//继承Thread类创建线程
class Thread_01 extends Thread{

    public void run(){
        System.out.print("Thread_01.run()");
        try {
            for(int i = 4; i > 0; i--) {
                System.out.println("Thread: " + Thread_01.currentThread().getName() + ", " + i);
                // 让线程睡眠一会
                Thread.sleep(50);
            }
        }catch (InterruptedException e) {
            System.out.println("Thread " +  Thread_01.currentThread().getName() + " interrupted.");
        }
        System.out.println("Thread " +  Thread_01.currentThread().getName() + " exiting.");
    }
}
public class ThreadTest {

    public static void main(String [] args){

        //获取对象，开启线程
        Thread_01 thread_01=new Thread_01();
        thread_01.start();
    }
}
```

注：由于java单继承的原因，所有继承Thread类创建线程局限性比较大，不建议使用。

### 3.2 实现Runnable接口，重写该类的run方法

该run()方法同样是线程执行体，创建Runnable实现类的实例，并以此实例作为Thread类的target来创建Thread对象，该Thread对象才是真正的线程对象。

```java
//实现Runable接口
class Thread_02 implements Runnable{
    public void run(){
        System.out.print("Thread_01.run()");
        try {
            for(int i = 4; i > 0; i--) {
                System.out.println("Thread: " + Thread.currentThread().getName() + ", " + i);
                // 让线程睡眠一会
                Thread.sleep(50);
            }
        }catch (InterruptedException e) {
            System.out.println("Thread " +  Thread.currentThread().getName() + " interrupted.");
        }
        System.out.println("Thread " +  Thread.currentThread().getName() + " exiting.");
    }

}

public class ThreadTest {

    public static void main(String [] args){
        //创建实现类
        Runnable runnable=new Thread_02();
        // 将runnable作为Thread target创建新的线程
        Thread thread=new Thread(runnable);
        // 调用start()方法，使线程进入就绪状态
        thread.start();
    }
}
```

### 3.3使用Callable和Future接口创建线程

创建Callable接口的实现类，并实现clall()方法。

使用FutureTask类来包装Callable实现类的对象，且以此FutureTask对象作为Thread对象的target来创建线程。

```java
//使用Callable和Future接口创建线程
class Thread_03 implements Callable<Integer>{
    /**
     * 与run()方法不同，call()方法具有返回值
     * @return
     */
    @Override
    public Integer call () {
        int sum = 0;
        for (int i = 0; i < 100; i++) {
            System.out.println("Thread:"+ Thread.currentThread().getName() +", i:"+ i);
            sum += i;
        }
        return sum;
    }
}

public class ThreadTest {

    public static void main(String [] args){
        // 创建myCallable对象
        Callable<Integer> callable = new Thread_03();
        // 使用FutureTask来包装myCallable对象
        FutureTask<Integer> ft = new FutureTask<Integer>(callable);
        System.out.println("Thread:"+ Thread.currentThread().getName());
        // FutureTask对象作为Thread对象的target创建新的线程
        Thread thread = new Thread(ft);
        // 线程进入到就绪状态
        thread.start();
        System.out.println("主线程for执行完毕...");
        try {
            // 取得新创建的新线程中的call()方法返回的结果
            int sum = ft.get();
            System.out.println("sum:"+ sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```



**总结：**

- 线程的启动而言，都是调用线程对象的start()方法，需要特别注意的是：不能对同一线程对象两次调用start()方法。
- 采用实现 Runnable、Callable 接口的方式创建多线程时，线程类只是实现了 Runnable 接口或 Callable 接口，还可以继承其他类。
- 使用继承 Thread 类的方式创建多线程时，编写简单，如果需要访问当前线程，则无需使用 Thread.currentThread() 方法，直接使用 this 即可获得当前线程。

## 5.重要方法的区分

### 5.1 sleep 与 与 wait 区别

1. 对于 sleep()方法，我们首先要知道该方法是属于 Thread 类中的。而 wait()方法，则是属于
Object 类中的。
2. sleep()方法导致了程序暂停执行指定的时间，让出 cpu 给其他线程，但是他的监控状态依然
保持者，当指定的时间（超时）到了又会自动恢复运行状态。
3. 在调用 sleep()方法的过程中，线程不会释放对象锁。
4. 而当调用 wait()方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此
对象调用 notify()方法后本线程才进入对象锁定池准备获取对象锁进入运行状态。

### 5.2 start 与 run 区别

1. start（）方法来启动线程，真正实现了多线程运行。这时无需等待 run 方法体代码执行完毕，
可以直接继续执行下面的代码。
2. 通过调用 Thread 类的 start()方法来启动一个线程， 这时此线程是处于就绪状态， 并没有运
行。
3. 方法 run()称为线程体，它包含了要执行的这个线程的内容，线程就进入了运行状态，开始运
行 run 函数当中的代码。 Run 方法运行结束， 此线程终止。然后 CPU 再调度其它线程。





修行之路艰辛

​                                                                                                                                  ---2020年3月   成都