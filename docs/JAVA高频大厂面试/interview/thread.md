# 线程基础

## 相关概念与创建线程

1. 进程与线程
   - 进程: 系统进行资源分配和调度的独立单位
   - 线程: 是程序执行的单元,执行路径,是程序使用CPU的基本单位
2. 线程:进程的调度需要各方面的调配(IO等),所以引入线程减少CPU空转时间
  >3个基本概念: 就绪, 执行, 阻塞
  >5种基本操作: 派生, 阻塞, 激活, 调度, 结束
  >多线程的存在不是提高程序执行速度,而是为了提高应用程序的使用率,程序的执行其实都是在抢CPU的资源,如果某一程序如果线程比较多就会有更高几率抢到CPU执行权
3. 并发,与并行
   
   并行(进程):
- 并行性是指同一时刻内发生两个或多个事件
- 并行是在不同实体上的多个事件

  并发(线程):
- 并发性是指同一时间间隔内发生两个或多个事件
- 并发是在同一实体的多个事件

4. Java实现多线程

   创建多线程有两种方法:

   - 继承Thread类重写run方法
   - 实现Runnable接口,重写run方法

 4.1 继承实现
 ```
 //创建一个类,继承Thread,重写run方法
 public class MyThread extends Thread {
     @Override
     public void run() {
         for(int x=0; x<100; x++) {
            sout(x);
         }
     }
 }

 //调用
 public class MyThreadDemo {
     public static void main(String[] args) {
         MyThread t1 = new MyThread();
         MyThread t2 = new MyThread();
         t1.start();
         t2.start();   //两个线程会不规则交替输出
     }
 }
```

4.2 实现Runnable接口重写run方法

```
public class MyRunnable implatements Runnable {
    @Override
    public void run() {
        for(int i=0; i<100; i++) {
            sout(i);
        }
    }
}

//调用方法会有所不同
public class MyRunnableDemo {
    public static void main (String[] args){
        MyRunnable my = new MyRunnable();

        Thread t1 = new Thread(my);
        Thread t2 = new Thread(my);
        t1.start();
        t2.start();
    }
}
```

5. Java实现多线程需要注意的细节

>run()与start()方法的区别
- run():仅仅是封装被线程执行的代码,直接调用就是普通的方法
- start():首先启动线程,然后由jvm去调用线程的run()方法

>jvm虚拟机的启动是多线程:启动时不仅启动main线程,至少还会启动垃圾回收线程


## Thraed线程类的API

记录一些常见的重要的方法

1. 设置线程名

- 调用Thread.currentThread().getName(),查看当前所运行的线程名称
- 如果未对任何线程命名会发现:主线程main,其他线程Thread-x
- 为线程命名,使用Thread的构造方法:new Thread(RunnablrImpl,StringName);
- 还可以通过Thread.setName(StringName);方法修改默认名称

2. 守护线程

守护线程是为其他线程服务的:垃圾回收线程就是守护线程

>当别的用户线程执行完,虚拟机就会退出,守护线程也就会被停止

注意点
- 在线程启动前设置为守护线程:setDaemon(boolean on)
- 使用守护线程不能访问共享资源(数据库,文件),应为它可能任何时候挂掉
- 守护线程产生的新线程也是守护线程

3. 线程优先级

线程优先级高仅仅表示线程获取CPU时间片的几率高,但不是一个确定因素,线程的优先级依赖于操作系统,Linux下优先级就会被忽略
> setPriority(int) //默认5,最小1,最大10,线程优先级不能大于组的优先级

4. 线程的生命周期(就绪,执行,阻塞)

- sleep(long millis)
> 调用sleep方法会进入计时等待状态,时间到了计入就绪状态而非运行状态

- yield()
>调用yield方法会先让别的线程执行,但不保证真正让出CPU(意思是:我有空,可以的话你们先执行,很少使用)

- join()
>调用join()方法会等待该线程执行完毕后才执行别的线程

- interrupt()
>之前用stop()方法,现在已经被设置为过时了,因为stop可以让一个线程终止掉另一个线程,这可能让对象处于不一致的状态,所以现在用interrupt()方法

>interrupt()不会真正终止一个线程,他仅仅是给这个线程发了一个信号告诉它,它应该结束了(也就是说Java设计者想要线程自己来终止,通过上面的信号就可以判断处理什么业务了,具体是中断还是继续由被通知的线程自己处理)
```
Thread t1 = new Thread( new Runnable(){
    public void run(){
        // 若未发生中断，就正常执行任务
        while(!Thread.currentThread.isInterrupted()){
            // 正常任务代码……
        }
        // 中断的处理代码……
        doSomething();
    }
} ).start();
//interrupt()仅仅是设置了一个标志有进程自己判断是否执行中断操作,中断一个运行的进程没有任何意义
//如果线程在阻塞状态调用了interrupt()方法会抛出异常,并且会退出阻塞,设置状态位false
```
>查看当前线程是否中断有两个方法

    1. 静态方法interrupted() // 会清楚中断标志位
    2. 实例方法isInterruped() //不会清除中断标志位

## 多线程基础必要知识

1. 多线程会遇到的问题:
> 线程安全问题(资源共享),性能问题(死锁)

2. 对象的发布与逸出

>发布(publish):使对象能够在当前作用域之外的代码使用
>逸出(escape):当某个不应该发布的对象被发布了

2.1. 常见逸出:

- 静态域逸出
    ```
    public static Set<Person> allperson;
    //public修饰的静态域相当于发布了这个对象,为调用init()时外面的代码已经可以使用allPersion;
    public void init(){
        xxx;
    }
    ```
- public修饰的get方法(private属性定义了public的get方法)
- 方法的参数传递(把对象传递给另外的方法)
- 隐式的this(在构造函数中对象还未初始化就已经调用了某一方法)

2.2. 安全的发布对象

  - 在静态域中直接初始化对象:public static Person = new Person();

    -静态初始化由JVM初始化阶段完成,JVM内部存在者同步机制

  - 对应的引用保存到volatile或者AtomicReferance引用中,保证了该对象引用的可见性和原子性

  - 由final修饰,该对象是不可变的,就一定是安全的

  - 由锁来保护

3. 解决多线程遇到的问题

解决安全问题:
- 无状态(没有共享变量)
- 使用final使该变量不可变(如果该对象引用了其它对象那么无论发布还是使用那么都要加锁)
- 加锁(内置锁,显示Lock锁)
- 使用JDK提供的类实现线程安全

    - 原子性(AtomicLong等)
    - 容器(ConcurrentHashMap)
    - ...

原子性(atomic包下):执行某一操作不可分割
可见性(volatile):多线程环境下变量修改时,所有线程都会知道该变量被修改了,但不保证原子性
> 一般来说volatile大多用于标志位上(判断操作),需要满足以下条件时使用

- 修改变量时不依赖变量当前值
- 该变量是可变的
- 访问变量时不需要加锁

4. 线程封闭(不共享成员变量)

5. 不变性(不可变对象一定是线程安全的)

- final仅仅是不能修改变量的引用,但引用里面的数据是可以修改的
    > 比如: final HashMap<Person> hashMap = new HashMap<>(); //其内部数据可以改变(add等),所以不是线程安全的
- 不可变对象在引用时还是要加锁的
    - 或者把里面的Person也设计成线程安全的类

- 设计不可变对象需要满足三个条件
    - 对象创建后状态就不能修改
    - 对象所有的域都是final修饰的
    - 对象是正确创建的(没有this逸出)

6. 线程安全委托:很对时候实现线程安全未必需要自己加锁自己设计,可以使用JDK提供的对象完成线程安全设计(concurrent.locks下的类)

## JAVA锁机制

1. synchronized

>synchronized是Java的一个关键字,它能够将代码块锁起来

- synchronized 是一种互斥锁:一次只能允许一个线程进入被锁住的代码块
- synchronized是一种内置锁/监视器锁:Java中每个对象都有一个内置锁(监视器锁,标记锁),而synchronized就是使用对象的内置锁来将代码块锁定的
- Java中的synchronized，通过使用内置锁，来实现对变量的同步操作，进而实现了对变量操作的原子性和其他线程对变量的可见性，从而确保了并发情况下的线程安全。

>synchronized底层是通过monitor对象,对象有自己的对象头存储了很多信息,其中一个信息表示被那个线程持有

- 当代码块执行完后会自动释放锁,当线程出现异常时其所持有的锁也会自动释放

2. Lock显式锁

>lock显式锁是JDK1.5后才出现的

- lock方式获取锁支持中断,超时不获取,是非阻塞的
- 提高了语义化手动加锁手动解锁
- 支持Condition条件对象
- 允许多个线程同时访问共享资源

>synchronized与Lock锁的性能差别不大,所以应优先选择synchronized,用到了lock锁的特殊性能才使用lock锁

3. 公平锁

>公平锁:线程按照它们发出请求的顺序来获取锁(非公平就是"插队")

Lock和 synchronized默认都是非公平锁,使用公平锁会消耗一些性能


## Lock锁子类

1. ReentrantLock(互斥锁)
要点:
- 比synchronized更有伸缩性
- 支持公平锁(相对公平)
- 最标准用法是在try前调用Lock方法,在finally代码块释放锁
```
class ReentrantLockDemo {
    private final ReentrantLock lock = new ReentrantLock();

    public void main(String[] args) {
        lock.lock;
        try {
            xxx;
        } finally {
            lock.unlock();
        }
    }
}
```

>ReentrantLock有三个内部类:Sync,NonfairSync,FairSync;(这些内部类都是AQS(AbstractQueuedSynchronizer)的子类,AQS是构建锁,同步器的框架)

2. ReentrantReadWriteLock(读写锁)

- 在读取数据时可以多个线程同时进入到临界区(被锁定的区域)
- 在写数据时无论是读线程还是写线程都是互斥的

要点:
>- 读锁不支持条件对象,写锁支持条件对象
>- 读锁不能升级为写锁,写锁可以降级为读锁
>- 读写锁也有公平和非公平模式
>- 读锁支持多个读线程进入临界区,写锁是互斥的
>条件对象:当一个线程获得锁却需要判断是否满足条件才能执行,此时就需要条件对象

- 两个内部类:WriteLock,ReadLock

## 线程池简单了解

>线程池可以看做线程的集合,在没有任务时线程处于空闲状态,当请求到来:线程池给这个任务一个空闲线程,任务完成后回到线程池中等待下次任务而不是销毁,这样就实现了线程的重用

### JDK提供的线程池

>JDK提供了Excutor框架来使用线程池,它是线程的基础,Excutor提供了一种"任务提交"与"任务执行"分离开的机制

>callable:可以理解为Runnable的扩展,Runnable没有返回值和不能抛出受检测的异常而Claaable可以

 ### ThreadPoolExecutor(常用的线程池) [TODO]

 











