# 菜鸟修行之路----java多线程与并发：线程同步



​         Java提供了多线程机制，通过多线程的并发运行可以提高系统资源的利用率，提高系统性能。但是也伴随很多问题例如：多线程造成数据混乱（多个不同线程同时操作一个变量或者资源），这个就是多线程里面比较重要的线程同步问题。

​       **线程同步**：执行多线程任务时，一次只能有一个线程访问共享资源，其他线程只能等待。

## 1.多线程造成数据混乱问题实例

模拟2个用户从银行取款，（2个线程同时操作银行账户余额）

代码示例：

```java
/**
 * 银行账户类
 */
class Mbank{
    private static int sum =2000;
    public static void take(int k){
        int temp=sum;
        //取款
        temp=temp-k;
        try {
            Thread.sleep((int)(1000*Math.random()));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        sum=temp;
        System.out.println("sum="+sum);
    }
}
/**
 * 用户取款的线程类
 */
class Customer extends Thread{
    public void run(){
        for(int i=1;i<=4;i++){
            Mbank.take(100);
        }
    }
}
/***
 *调用线程的主类
 */
public class ThreadData {
    public static void main(String [] args){
        Customer customer1=new Customer();
        Customer customer2=new Customer();
        customer1.start();
        customer2.start();
    }
}
```

运行结果：

![1584151373930](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1584151373930.png)

通过上诉结果可以得知，由于多线程操作，使得银行账户数据出现错误（一直在取款，但是余额没有一直减少）。

## 2.线程同步的实现

### 2.1 synchronized 关键字（同步锁）

​        实现线程同步的核心思路，就是**互斥**（一次只能允许一个线程访问共享资源）。

​       在java语言中，采用监视器（MiW）实现互斥机制，在多线程任务下，一段时间内只有一个线程拥有监视器，也就是只有拥有监视器的线程才能访问相应资源，并且锁定资源不让其他线程使用。

​      所有Java对象都有与他们相关的隐含监视器，并且调用对象有Synchronized修饰的方法进入对象监视器。

​     当只要有一个线程进入有Synchronized修饰的方法，同类其他线程就不能进入该方法，必须等待。

​      使用同步锁的方式一般有2种：同步方法，同步代码块

#### 2.1.1同步方法

**使用Synchronized修饰方法（静态方法和非静态方法均可）**

例如如上文种多线程问题举例中：

```java
/**
 * 银行账户类
 */
class Mbank{
    private static int sum =2000;
    //使用Synchronized修饰方法,进行线程同步
    public synchronized static void take(int k){
        int temp=sum;
        //取款
        temp=temp-k;
        try {
            Thread.sleep((int)(1000*Math.random()));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        sum=temp;
        System.out.println("sum="+sum);
    }
}
```

注意：

**当使用synchronized修饰静态方法时，线程此时获得的锁对象是类的Class对象（堆内存中只有唯一一个Class对象，因为Class对象是在类加载时产生的，而类加载只执行一次），因此会锁住整个类，其他线程无法访问该类的同步静态方法，但是可以访问非同步的方法**

#### 2.1.2 同步代码块

使用synchronized修饰需要同步的代码块 。

例如如上文种多线程问题举例中：

```java
/**
 * 银行账户类
 */
class Mbank{
    private static int sum =2000;
    //使用Synchronized修饰方法,进行线程同步
    public  static void take(int k){
        int temp=sum;
        synchronized (this){
            //取款
            temp=temp-k;
        }
        try {
            Thread.sleep((int)(1000*Math.random()));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        sum=temp;
        System.out.println("sum="+sum);
    }
}
```



**总结：**

同步通常会导致线程堵塞，使的程序执行效率变低。所有在使用Synchronized同步锁时，尽量同步关键代码，不用同步整个方法。



Synchronized同步锁经典应用：

- 解决单例模式饿汉式单例写法的线程不安全问题。（具体实现参见《设计模式之单例模式》）
- 解决常用集合类中（ArrayList、LinkedList、HashMap等）的线程不安全问题。

### 2.2 重入锁reentrantLock()

**ReentrantLock特性**：

- 可重入：一个线程已经获得了锁，其内部还可以多次申请该锁成功
- 可通过构造参数设置时公平锁还是非公平锁
- 需要明文释放锁，而synchronized是自动释放的
- 可响应中断
- 可在获取锁是设置超时时间
- 通知队列

**简单示例**：

```java
/**
 * 银行账户类
 */
class Mbank{
    private static int sum =2000;
    //声明重入锁
    private Lock lock=new ReentrantLock();
    public  static void take(int k){
        int temp=sum;
        //加锁
        lock.lock();
        //取款
        temp=temp-k;
        try {
            Thread.sleep((int)(1000*Math.random()));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //释放锁，需要主动释放
        lock.unlock();
        sum=temp;
        System.out.println("sum="+sum);
    }
}
```

### 2.3 局部变量ThreadLocal

使用thread管理变量，每一个使用该变量的线程都会获得该变量的副本，这样每一个线程可以随意修改自己的变量副本，不会对其他线程产生影响。

简单示例：

```java
/**
 * 银行账户类
 */
class Mbank{
    //创建一个线程本地变量ThreadLocal
    private static ThreadLocal<Integer> sum=new ThreadLocal<Integer>(){
        protected Integer initialValue(){
            return 2000;
        }
    };
    public static void take(int k){
        //取款,设置线程副本中的值
        sum.set(sum.get()-k);
        try {
            Thread.sleep((int)(1000*Math.random()));
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //输出线程副本中的值
        System.out.println("sum="+sum.get());
    }
}
```

## 3. 管程以及wait()、notify()

管程：拒绝线程访问和通知线程允许访问某个线程的一个对象。

管程（也可以称为互斥锁机制）是一对并发同步机制，包括用于分配一个特定的共享资源或者一组共享资源的数据和方法。

​       java为每一个拥有Synchronized方法的实例对象提供了唯一的管程，。为了完成资源分配，线程必须调用管程入口或者拥有互斥锁。管程入口就是Synchronized方法入口。

​      管程实现严格的互斥，同一时刻只允许一个线程进入。如果调用管程入口的线程发现线程已经被分配，管程中的这个线程将调用等待操作wait()。调用该方法后，该线程放弃管程的使用权，在管程外部等待。当已经分配资源归还给系统后，该线程调用一个通知操作notify()，通知系统允许其中一个正在等待的线程获取管程并且得到资源。被通知线程是排队的。（也可以调用notifyAll()方法通知所有线程来竞争管程，其中一个获得，其余继续等待）

注：本部分内容设计操作系统基础相关知识。

### 3.1使用synchronize以及wait()、notify() /notifyAll()实现生产者消费者模式

**经典的并发问题：生产者、消费者的java代码实现**

```java
/**
 * 使用synchronize wait notify/notifyall实现生产者消费者模式
 */

class ShareDataV1 {
    //定义原子操作类，防止出现线程不安全问题。
    public static AtomicInteger atomicInteger = new AtomicInteger();
    public volatile boolean flag = true;
    public static final int MAX_COUNT = 10;
    public static final List<Integer> pool = new ArrayList<>();
    //生产者
    public void produce() {
        // 判断，生产，通知
        while (flag) {
            // 每隔 1000 毫秒生产一个商品
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
            }
            //线程同步
            synchronized (pool) {
                //池子满了，生产者停止生产
                //TODO 判断
                while (pool.size() == MAX_COUNT) {
                    try {
                        System.out.println("pool is full, wating...");
                        pool.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                //生产
                pool.add(atomicInteger.incrementAndGet());
                System.out.println("produce number:" + atomicInteger.get() + "\t" + "current size:" + pool.size());
                //通知
                pool.notifyAll();
            }
        }
    }
    //消费者
    public void consumue() {
        // 判断，消费，通知
        while (flag) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            synchronized (pool) {
                //池子为0，消耗完了，消费者停止消耗
                //TODO 判断
                while (pool.size() == 0) {
                    try {
                        System.out.println("pool is empty, wating...");
                        pool.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                //消费
                int temp = pool.get(0);
                pool.remove(0);
                System.out.println("cousume number:" + temp + "\t" + "current size:" + pool.size());
                //通知
                pool.notifyAll();
            }
        }
    }

    public void stop() {
        flag = false;
    }
}

public class ProducerConsumer_V1 {

    public static void main(String[] args) {
        ShareDataV1 shareDataV1 = new ShareDataV1();
        new Thread(() -> {
            shareDataV1.produce();
        }, "AAA").start();

        new Thread(() -> {
            shareDataV1.consumue();
        }, "BBB").start();

        new Thread(() -> {
            shareDataV1.produce();
        }, "CCC").start();

        new Thread(() -> {
            shareDataV1.consumue();
        }, "DDD").start();
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        shareDataV1.stop();
    }
}
```





修行之路艰辛，与君共勉

​                                                                                                                                        ----2020年3月 成都