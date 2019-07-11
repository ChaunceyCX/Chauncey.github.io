# java面试题整理

## 基础

1. 简单说下跨平台?

>由于不同操作系统之间所支持的指令集存在差异,所以在操作系统上加个虚拟机提供统一的接口来屏蔽系统之间的差异

2. Java8中基本数据类型?
  
| 数据类型 | 字节 | 默认值   | 包装类型  |
| -------- | ---- | -------- | --------- |
| byte     | 1    | 0        | Byte      |
| short    | 2    | 0        | Short     |
| int      | 4    | 0        | Integer   |
| long     | 8    | 0        | Long      |
| float    | 4    | 0.0f     | Float     |
| double   | 8    | 0.0d     | Double    |
| char     | 2    | '\u0000' | Character |
| boolean  | 4    | false    | Boolean   |

- 自动装箱与自动拆箱:
  > 自动装箱: new Integer(1),底层调用:Integer.valueOf(1),得到的是一个对象
  >
  > 自动拆箱: int i = new Integer(1) ; 底层调用的是i.intValue(); 得到的是int

- 基本数据类型与包装类型的区别:
  1. 声明方式不同;
  2. 存储方式以及存储位置不同:基本数据类型存储在栈中,而包装类型存储在堆中
  3. 初始值不同:包装类型的初始值为null

3. ==和equals的区别?

> == 比较的是两个引用在内存中是不是同一个对象
> 
> equals用来比较某些特征,String的equals是重写后的,实现比较两个对象内容是否相等

4. String/StringBuffer/StringBuilder的区别
   1. 数据可变和不可变:
       - String底层使用一个不可变字符数组:private final char value[]; 所以它的内容不可变;
       - StringBuffer和StringBuilder都继承了AbstractStringBuilder底层使用的是可变数组:char[] value;
   2. 线程安全:
        - StringBuilder是线程不安全的,效率极高;而StringBuffer是线程安全的,效率极低,StringBuffer的append()方法有同步锁,而StringBuilder没有
    3. 相同点:
        - StringBuffer与StringBuilder有共同的父类:AbstractStringBuilder
    4. 操作可变字符串的速度: StringBuilder>StringBuffer>String

5. 说一下Java中的集合:
    - Collection下:List系(有序,元素可重复),Set系(无序,元素不重复)
    >Set根据equals和hashcode判断重复,一个对象要存入set中必须重写equals和hashcode
    - Map下:HashMap线程不同步; TreeMap线程同步
    - Map系列是对Collection的补充两者没有任何关系

6. ArrayList和LinkedList区别?
   
    - ArrayList基于动态数组的数据结构,LinkedList基于链表的数据结构
    - 由于两者数据结构的区别:对于随机访问(get和set)ArrayList优于LinkedList(因为LinkedList要移动指针),对于新增和删除,LinkedList占优势,因为链表只需要连接相应指针

    ### ArrayList与LinkedList源码分析:
    [ArrayList源码解析](/java/ArrayListSource )

    [LinkedList源码解析](/java/LinkedListSource )

7. 集合中ConCurrentModificationException异常出现的原因
   ```java
   public static void main (String[] args) {
       ArrayList<Integer> list = new ArayList<>();
       list.add(2);
       Iterator<Integer> iterator = list.iterator();
       while(iterator.hasNext()) {
           Integer i = iterator.next();
           if(i==2) {
               list.remove(i); //会导致modCount和expectedModCount的值不一致,抛出异常
           }
       }
   }

   //正确写法,调用Iterator的remove方法
   public static void main(String[] args)  {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(2);
        Iterator<Integer> iterator = list.iterator();
        while(iterator.hasNext()){
            Integer integer = iterator.next();
            if(integer==2)
                iterator.remove();   //注意这个地方
        }
    }
    ```

8. HashMap,HashTable,ConcurrentHashMap的区别

    - 相同点:
        1. HashMap和HashTable都实现了Map接口
        2. 都是key-value存储
    - 不同点:
        1. HashMap可以把null作为key或者value,HashTable不可以
        2. HashMap线程不安全,效率高,HashTable线程安全,效率低
        3. HashMap的迭代器(Iterator)是fail-false迭代器,HashTable的(Enumerator)不是的
            >fail-false : 最快时间把错误抛出而不是让程序执行

    - 能否既保证线程安全又提高效率        
        - 使用ConCurrentHashMap,它是HashTable的替代,比HashTable的扩展性好
        - TODO
          -  [ ] 三者源码

    - 能否让HashMap同步?
    > HashMap可以通过以下语句同步: Map m = Collections.synchronizedMap(hashMap);

9. 拷贝文件的工具类使用的是字节流还是字符流?

    >使用的是:因为所有文件都是以二进制形式存在,考虑到通用性使用字节流

    - 字节流:传递字节(二进制)
    - 字符流:传递字符

10. 两个对象的hashCode相同,则equals也一定为true,对吗?
    
    >不对:因为对象可以重写hashCode()方法让其返回值相同
    - TODO
      - [ ] 非特意情况是否一定相同??

    >两个对象equals为true,则hashCode()也一定相同吗?
    > 答: 不相同,同理,不重写hashCode()的话相同,重写的话,就会出现不相等的情况

11. java线程池创建?
    >使用Executors,其提供了4种创建方式

    1. newFixedThreadPool():创建固定大小的线程池
    2. newCachedThreadPool():创建无限大小的线程池,线程池中数量不固定,可根据需要手动修改
    3. newSingleThreadPool():创建只有一个线程的线程池
    4. newScheduledThreadPool():创建固定大小的线程池,可以延时或定时执行任务

### 线程池作用
    
- 限制线程个数,避免线程过多导致系统运行缓慢或崩溃
- 避免频繁的创建和销毁,节约资源,响应更快


12. Math.round(-2.5)等于多少?
    >它不是四舍五入,口诀:原数加0.5后向下取整,所以等于-2
    >-2.6等于-2;2.6等于3

13. 面向对象六大原则
    1. 单一职责原则(SRP)
>让每个类只专心处理自己的方法
    2. 开闭原则(OCP)
>软件中的对象(类.模块,函数等)应对于扩展是开放的.但是对于修改是关闭的
    3. 里氏替换原则(LSP)
>子类可以扩展父类但是不能改变父类原有的功能
    4. 依赖倒置原则(DIP)
>应该通过调用接口或者抽象类,而不是调用实现类
    5. 接口隔离原则(ISP)
>把接口分成依赖最系的最小接口,实现类中不能有不需要的方法
    6. 迪米特原则(LOD)
>高内聚,低耦合

14. static和final的区别

| 关键词 | 修饰物 | 影响                                               |
| ------ | ------ | -------------------------------------------------- |
| final  | 变量   | 分配到常量池中,程序不可改变其值                    |
| final  | 方法   | 子类中将不能被重写                                 |
| final  | 类     | 不能被继承                                         |
| static | 变量   | 分配在内存堆上.引用都会指向这一地址,不会被重新分配 |
| static | 方法块 | 虚拟机优先加载----------???                        |
| static | 类     | 可以直接通过类来调用而不需要new------???           |
    - TODO
      - [ ] ???处好像有问题

15. String s = "xxx"; 与 String s = new String("xxx");的区别?

- String s = "xxx":JVM会首先查看字符串常量池中有没有"xxx"对象,没有就会开辟内存空间创建一个,如果有的话,就会直接将常量池中相应对象的地址返回给栈(没有new关键字,就没有堆的操作);
- String s = new String("xxx"):可能会创建一个对象,也可能创建两个,如果没有就会创建一个,然后再在堆上创建一个String对象;

- TODO
  - [ ] 深究一下,好像不是很清楚

1.  引用类型占用几个字节?

>在64位平台上占用8个字节,在32位平台上占4字节

17. 什么情况下"+"会变成连字符?

> 加号前面有字符时
```java
System.out.println( ( (1<3)?"a":"b" ) + ( 3+4 ) );
//控制台会输出:a7
```

18. java中switch可以使用的数据类型(JDK 1.8)
    1. char
    2. byte
    3. short
    4. int
    5. Character
    6. Byte
    7. Short
    8. Integer
    9. String
    10. enum
>记忆方法: 基本类型中没有浮点类型和长类型long,相应的包装类型也没有外加String和enum

19. 4&5 , 4^5 , 4&5>>1 

- TODO
    - [ ] 各个运算符以及优先级整理

20. java类为什么要实现序列化(实现Serializable接口)
>为了网络传输或者持久化
>序列化就是将对象的信息转换为可以储存或者传输的形式的过程
>其它的序列化方式:
>>- Json序列化
>>- FastJson序列化
>>- ....

21. JVM垃圾处理方法

    - 标记清除算法(老年代区域)
    该算法分为"标记"和"清除"两个阶段:首先标记出所有需要回收的对象(可达性分析), 在标记完成后统一清理掉所有被标记的对象.
    该算法会有两个问题：
        1. 效率问题，标记和清除效率不高。
        2. 空间问题: 标记清除后会产生大量不连续的内存碎片, 空间碎片太多可能会导致在运行过程中需要分配较大对象时无法找到足够的连续内存而不得不提前触发另一次垃圾收集。
    所以它用于垃圾不太多的区域,比如老年代.

    - 复制算法(新生代)
    https://mp.weixin.qq.com/s?__biz=MzI4Njc5NjM1NQ==&mid=2247487745&idx=1&sn=29e33490f20a38f65059b03aaceed3cf&chksm=ebd62e2ddca1a73b1c662cc4fa3a5b651cb271d2601063db190e0fded8144fc054990da8ef0b&scene=21#wechat_redirect



## 线程相关

1. 创建线程的方法:


- 继承Thread类,重写run()方法;

```java
    public class MyThread extends Thread {

        @Override
        public void run() {
            while(! interrupted()) { // @@1
                //do sth
            }
        }

        public static void main(String[] args) {
            MyThread t1 = new MyThread("t1");
            t1.start();


            t1.interrupt(); //@@2 
            // 想要中断线程时最好结合 @@1 和 @@2 调用interrupt()方法实现
            // 禁止调用stop();方法,该方法不会释放占用的资源
        }
    }

```   

- 实现Runnable接口,重写run()方法

```java
    //Runnable只是用来修饰所执行的任务的,它不是一个线程对象,想要启动一个Runnable任务必须将它放到线程对象里面
    public class MyThread implements Runnable {
        @Override
        public void run() {
            //do sth
        }

        public static void main(String[] args) {
            Thread t1 = new Thread(new MyThread());
            t1.start();
        }
    }
```

- 匿名内部类创建线程对象

```java
    public static void main(String[] args) {
        //创建无参数线程对象
        new Thread() {
            @Override
            public void run() {
                //do sth
            }
        }.start();

        //创建带线程任务的线程对象
        new Thread( new Runnable() {
            @Override
            public void run() {
                //do sth
            }
        }).start();
    }
```

- 创建带返回值的线程

```java
    public class MyThread implements Callable {
        @Override
        public Object call() throws Exception {
            int result = 1;
            //do sth
            return result;
        }

        public static void main(String[] args) {
            MyThread t1 = new MyThread();
            FutureTask<Integer> task = new FutureTask<Integer>(t1);
            Thread thread = new Thread(task);
            thread.start();

            Integer result = task.get();
        }
    }
```

- 定时器Timer创建

```java
    public void main(String[] args) {
        Timer timer = new Timer();
        timer.schedule(new TimerTask() {
            @Override
            public void run() {
                //do sth
            }
        },0,1000); //延迟0,周期1s
    }
```

- 线程池创建线程

```java
    public static void main(String[] args) {
        //创建一个有十个线程的线程池
        ExecutorService threadPool = Executors.newFixedThreadPool(10);
        long threadPoolUseTime = System.currentTimeMillis();
        for (int i=0; i<10; i++) {
            threadPool.execute(new Runnable() {
                @Override
                public void run() {
                    //do sth
                }
            });
        }
        long threadPoolUseTime1 = System.currentTimeMills();
        //多线程用时
        threadPoolUserTime1-threadPoolUseTime;
        //销毁线程池
        threadPool.shutdown();
    }
```

- 利用Java8新特性stream实现并发

```java
    public static void main(String[] args) {
        List<Integer> values = Arrays.asList(10,20,30,40);
        //parallel: 平行的,并行的
        //所以此行代表并发执行sum();
        int result = values.parallelStream().mapToInt(p->p*2).sum();

        //乱序输出说明是并发执行的
        values.parallelStream().forEach(p-> System.out.println(p));
    }
```

2. sleep() 与 wait() 的区别?

    >sleep()会让线程进入阻塞状态,时间结束继续运行(不会释放锁)
    >wait()会让线程释放锁进入等待队列,notify/notifyall后会进入就绪队列
