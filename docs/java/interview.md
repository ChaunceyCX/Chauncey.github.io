# java面试题整理

## 基础

1. 简单说下跨平台?

>由于不同操作系统之间所支持的指令集存在差异,所以在操作系统上加个虚拟机提供统一的接口来屏蔽系统之间的差异

2. Java的8中基本数据类型?
   
    | 数据类型 | 字节 | 默认值 | 包装类型 |
    | -----   | --- | ----- | ----    |
    | byte    | 1   |   0   | Byte    |
    | short   | 2   |   0   | Short   |
    | int     | 4   |   0   | Integer |
    | long    | 8   |   0   | Long    |
    | float   | 4   | 0.0f  | Float   |
    | double  | 8   | 0.0d   | Double |
    | char    | 2   |'\u0000'| Character|
    | boolean | 4   |false   | Boolean |

- 自动装箱与自动拆箱:
  > 自动装箱: new Integer(1),底层调用:Integer.valueOf(1),得到的是一个对象
  > 自动拆箱: int i = new Integer(1) ; 底层调用的是i.intValue(); 得到的是int
- 基本数据类型与包装类型的区别:
  1. 声明方式不同;
  2. 存储方式以及存储位置不同:基本数据类型存储在栈中,而包装类型存储在
  3. 初始值不同:包装类型的初始值为null

3. ==和equals的区别?

> == 比较的是两个引用在内存中是不是同一个对象
> equals用来比较某些特征,String的equals是重写后的,实现比较两个对象内容是否相等

4. String/StringBuffer/StringBuilder的区别
   1. 数据可变和不可变:
       - String底层使用一个不可变字符数组:private final char value[]; 所以它的内容不可变;
       - StringBuffer和StringBuilder都继承了AbstractStringBuilder底层使用的是可变数组:char[] value;
   2. 线程安全:
        - StringBuilder是线程不安全的,效率极高;而StringBuffer是线程不安全的,效率极低,StringBuffer的append()方法有同步锁,而StringBuilder没有
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
    - 由于两者数据结构的区别:对于随机访问(get和set)ArrayList由于LinkedList(因为LinkedList要移动指针),对于新增和删除,LinkedList占优势,因为链表只需要连接相应指针

   ### ArrayList与LinkedList源码分析:
   

## 线程相关

1. sleep() 与 wait() 的区别?

    >sleep()会让线程进入阻塞状态,时间结束继续运行(不会释放锁)
    >wait()会让线程释放锁进入等待队列,notify/notifyall后会进入就绪队列
