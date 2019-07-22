# 集合相关补充以及深入

### 1. Arraylist与Vector的区别

- 相同点:这两个类都继承了List接口(List接口继承了Collection接口),他们都是有序集合,都可以通过索引取得某个元素,都是动态数组
- 不同点:
  - 同步性:Vector是线程安全的(多线程),也就是说它是线程同步的,ArrayList是线程不安全的(单线程)
  - 数据增长:ArrayList与Vector都有一个初始容量大小,当存储进的容量达到或者超过时Vector增长原来的一倍,ArrayList"平均增长0.5倍"

### 2. Collection和Collections的区别?

- Collection是集合类上的接口,继承它的接口主要有List和Set
- Collections是针对集合类的一个工具类,它提供一系列静态方法实现对各种集合的搜索\排序\线程安全化等操作

### 3. 
