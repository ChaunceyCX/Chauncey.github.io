# ArrayList源码分析

## 线程安全性

>对ArrayList的操作一般分为两个步骤,改变长度和操作元素,所以不能保证原子性,因此在多线程下是不安全的

## add()方法,在数组末尾添加元素

```
//size为集合实际大小
// size+1 经常被考
public boolean add(E e) {
    ensureCapacityInternal (size + 1);
    elementData[size+1] = e;
    return true;
}

public void ensureCapacityInternal (int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

//计算容量
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

//计算完容量后，进行确保容量可用：(modCount不用理它，它用来计算修改次数)
//如果size+1 > elementData.length证明数组已经放满，则增加容量，调用grow()
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

//增加容量:
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}

```

## 删除元素
```
public E remove(int index) {
    // 检测index是否合法
    rangeCheck(index);
    // 数据结构修改次数
    modCount++;
    E oldValue = elementData(index);

    // 记住这个算法
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
}
```

### System.arraycopy()

参数解析: 
    - 原数组
    - 原数组的起始位置
    - 目标数组
    - 目标数组的起始位置
    - copy的原数组的长度
  
## ArrayList的优缺点

1. 优点:
   - 因为底层是数组,所以修改查询效率高
   - 可自动扩容,size>>1 ( 平均扩容1.5倍???)

2. 缺点:
    
   - 插入和删除效率不高
   - 线程不安全(因为操作元素时要考虑size所以不是违背了线程安全的原子性)