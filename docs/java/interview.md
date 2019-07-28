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

22. 一个" .Java "源文件中是否可以包含多个类?

>可以有多个类,但是只能有一个public的类(或者内部类),但是public类名需要与文件名一致

23. Java有没有"goto"?

>作为Java保留字,现在还没偶在Java中使用

24. 说说&和&&的区别?

- 两者都可以作为逻辑与的运算符,符号两边都为true是,整个运算结果才为true
- &&具有短路功能,所以当符号左边为false时,结果就为false不会再去看符号右边
- &还可以作为位运算符,当两边都不是boolean类型时就会做按位与运算(用法:通常取一个整数的低四位时会对该整数与0X0f进行按位与运算)

25. 在java中如何跳出当前的多重嵌套循环?

    1. 外层循环定义标号

```java
@Test
    public void testBreak () {
        tag :
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 2 ; j++) {
                System.out.println(i+"++" +j);
                if (j==1)
                    break tag;
            }
        }
    }
```

    2. 让外层循环条件表达式受内层循环控制

```java
@Test
    public void testBreak () {
        boolean tag = true;
        for (int i = 0; i < 3&&tag; i++) {
            for (int j = 0; j < 2 ; j++) {
                System.out.println(i+"++" +j);
                if (j==1) {
                    tag = false;
                    break;
                }
            }
        }
    }
```

26. java1.7之后switch支持String了

27. short s1 = 1； s1 = s1 +1； s1 += 1；解读？

- s1 = s1 + 1： s1+1时会自动提升运算类型为int，但是赋值给s1时编译器将报告需要强转类型错误
- s1+=1：由于+=是Java语言定义的运算符所以java编译器会进行特殊处理，因此可以正确编译

28. char能不能存储一个中文汉字？

>char类型是用来存储Unicode编码字符的,unicode字符集包含了汉字所以可以存储汉字(注意有些汉字不在unicode里面就不能存储了)

29. 最有效率计算2乘以8?
    
    > 2<<3(2左移3位),一个数左移几位代表乘以2的几次方

30. final修饰的变量,其对应的引用地址不能变,但是引用所指向的对象属性可以变

```java
final StringBuffer a = new StringBuffer("xxx");

//报错
a= new StringBuffer("");
//正确
a.append("aaa");
```

31. 静态变量与实例变量的区别?

- 语法定义上的区别: 静态变量要加static关键字
- 程序运行时区别: 实例变量属于对象的属性只有创建了对象实例变量才会分配空间.静态变量不属于对象,属于类,只要程序加载了类的字节码,不用创建任何实例对象,就会给静态变量分配内存空间,静态变量可以直接通过类名引用

```java
//无论创建多少实例对象,永远分配了一个静态变量空间,所以每次创建一个对象staticVar就会较上一次加一
public class VariantTest {
    public static int staticVar = 0;
    public int instanceVar = 0;
    public VariantTest(){
        staticVar++;
        instanceVar++;
    }
}
```

32. 是否可以从一个static方法访问一个非static方法?

>不可以,因为非静态方法要与对象关联在一起,要创建一个对象后访问该对象上的非静态方法

33. Math.round(),Math.ceil(),Math.floor()用法?

>Math.round():加0.5向下取整;
>Math.ceil():向上取整;Math.ceil(11.3)=12,ceil(-11.3)=-11
>Math.floor():向下取整;Math.floor(11.6)=11,-11.6=-12

34. Overload和Override的区别?

- Overload是重载的意思,Override是覆盖的意思,也就是重写
- 重载表示一个类中可以有多个名称相同的方法,但这些方法的参数类型以及个数不同(如果参数不同返回值也可以不同,如果参数相同)
- 覆盖表示子类继承父类重写父类的方法,覆盖的方法参数一致返回值一致

35. 接口是否可以继承接口?抽象类是否可以实现接口?抽象类是否可以继承具体类?抽象类中是否可以有静态main方法?

- 都可以
- 抽象类与普通类的唯一区别就是不能创建实例对象和允许有abstract方法

36. 内部类可以引用它的包含类成员吗?有什么限制?

>完全可以,但是如果是静态内部类只能引用静态成员

```java
class Out {
    static int x;
    static class Inner {
        void test() {
            sout(x);
        }
    }
}
```

37. [集合相关](/java/interview-collection)

38. [线程相关](/java/interview-thread)

39. String s = "a" + "b" + "c" + "d";一共创建了多少对象?

```java
String s1 = "a";
String s2 = s1 + "b";
String s3 = "a" + "b";
Sout(s2=="ab"); //false
sout(s3=="ab"); //true
```
上面的例子说明编译器在编译时对字符串常量直接相加的表达式优化,直接在编译时去掉加号,直接将其编译成一个常量;
所以题目只创建了一个String对象;

40. try{}里面有一个return语句,那么紧跟后面的finally{}还会不会执行?

>在return中间执行

```java
public class Test　{
    public static void main(String[] args){
        System.out.println(test());
    }

    test() {
        int x = 1;
        try{
            return x;
        }
        finally{
            ++x;
        }
    }
}
//最后会在控制台输出1
//主函数调用子函数并得到结果的过程，好比主函数准备一个空罐子，当子函数要返回结果时，先把结果放在罐子里，然后再将程序逻辑返回到主函数。所谓返回，就是子函数说，我不运行了，你主函数继续运行吧，这没什么结果可言，结果是在说这话之前放进罐子里的。
```

41. final,finally,finalize的区别

- final:用于声明属性,方法,类,分别表示属性不可变,方法不可覆盖,类不可被继承,内部类要访问局部变量,局部变量必须定义成final类型
- finally:是异常处理结构的一部分
- finalize:是Object类的一个方法,在GC执行时会调用被回收对象的此方法,可以覆盖(重写)此方法提供垃圾收集时的其它资源回收例如关闭文件,但是JVM不保证此方法总是被调用

42. error和exception有什么区别?

- error表示恢复不是不可能但是很困难情况下的一种严重问题(比如:内存溢出),
- exception表示一种设计或实现问题

43. 简单说说java异常处理机制的简单原理和应用

>java使用面向对象的方式来处理异常,它把程序中发生的每个异常也都分别封装到一个对象中

>java对异常进行了分类,不同类型的异常用不同的Java类表示,所有异常的根类为:java.lang.Throwable,Throwable下面又派生了两个子类:Error和Exception
>Exception又分为系统异常和普通异常:
>>- 系统异常是软件本身缺陷导致问题,也就是软件开发者考虑不周导致的,软件使用者无法克服和恢复这种问题(例如:数组下标越界,空指针,类转换异常等)
>>- 普通异常是运行环境的变化或异常所导致的问题(例如:网络断线,硬盘空间不足等)

44. Java中堆和栈有什么区别?

堆和栈属于不同的内存区域
- 栈:存储方法和局部变量,程序进入一个方法时会为这个方法分配一个存储空间用于存储这个方法内部的局部变量,当这个方法结束时会释放内存空间
- 堆:保存对象,由GC来管理
- 方法中的变量被final修饰时,变量会存放于堆中

```java
Student a = new Student();
//new Student(),存储在堆上
//a获得new Student();的堆地址,存储在栈上

Student b = a;
//a和b都存储在栈上并且它们指向的对象相同(堆内存地址)

```

45.  描述一下JVM加载class文件的原理机制

>JVM中类的装载由ClassLoader和它的子类来实现

46. 垃圾回收的基本原理是什么?

>通常,GC采用有向图的方式记录和管理堆中的所有对象,通过这种方式确定那些对象是可达的,然后进行回收
>可以使用System.gc(),通知GC运行,但是java语言规范不保证GC一定运行

47. throw和throws的区别?

- throw用于抛出一个异常对象
- throws方法抛出异常然后在throws子句中处理

48. Java中存在内存泄露吗?

>内存泄漏:指一个对象或变量不在被程序引用却一直占着内存

- java中存在内存泄露的情况:因为GC的存在一般不会内存泄漏,但是如果长生命周期的对象持有短生命周期的对象就有可能发生内存泄漏

49. Servlet的生命周期?

>Servlet被服务器***实例化***后,容器运行其***init()***方法,请求到达时运行其***service()***方法service方法自动派遣运行与请求对应的doXXX方法***(doGet,doPost)***等,当服务器决定将实例销毁时会调用其***destroy()***方法

50. Servlet API中的forword()与redirect()的区别?

    1. 从地址显示来说:
        >forword是服务器内部请求资源,服务器直接把forword指向的URL资源返回给浏览器,浏览器以为不知道当前资源对应的URL已经变了,所以它的地址栏还是原来的地址
        >redirect是服务器根据逻辑,发送一个状态码告诉浏览器需要重新请求另一个URL,所以地址栏会变
    2. 从数据共享来说:
        >forword整个流程下来只发生了一次request和respose,所以请求资源和转发资源可以共享request
        >redirect显然不能共享request,另redirect可以重定向到任意甚至非本站的资源
    3. 应用场景:
        - forword应用于用户登录时根据角色权限转发到相应模块
        - redirect应用于用户注销时回到主页面和跳转到其它网站

51. request的getAttribute()和getParameter()的区别
    1. getParameter()是通过类似post,get等方式传入的数据
    2. 

