# ArrayList, LinkedList, Vector 简单分析

## ArrayList

底层实现为数组，默认初始容量为10。每次向其中增加元素，都要进行检查是否需要扩容和重新copy操作。因此，我们在基本清楚数据量的情况下，尽量赋予初始容量，避免扩容时候带来的数据拷贝问题。

**注意** 

- ArrayList属于线程不安全，为不同步的。
- 每次扩容为1.5倍，原始大小+原始大小>>1，这个扩容方式最多浪费33%的内存，即: 0.5/1.5=0.33；若扩容太小，则需要时长进行arrayCopy操作。
- 想要保证同步，可以使用```Collections.synchronizedList(new ArrayList(...))```

### 扩容策略(源码追踪)
```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // 确保容量是否放得下，放不下需要扩容
    elementData[size++] = e;    // 将新元素直接放置在元素最后
    return true;
}


private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity)); // 默认calculateCapacity计算出来的值为10，如果为第一次添加元素。否则为minCapacity的大小
}

private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // 原本容器中已满，新添加的值需要进行扩充
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1); // 关键代码，1.5倍扩容，计算得到新的大小
    if (newCapacity - minCapacity < 0)  // 扩容之后还小于原始大小？？？
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)   // 是否内存放得下
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity); // 将旧容器中的元素拷贝到新的容器中
}

```


## LinkedList

底层实现为双向链表，插入更加优于ArrayList，但是随机访问略微逊色，需要从头到位进行迭代。**线程不安全**

### 扩容策略(源码追踪)
```java
// LinkedList的其中一个变量，记录了链表的大小
transient int size = 0;

// 第一个节点
transient Node<E> first;

// 最后一个节点
transient Node<E> last;

// 内部节点
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}

public boolean add(E e) {
    linkLast(e);
    return true;
}

void linkLast(E e) {
    final Node<E> l = last;
    final Node<E> newNode = new Node<>(l, e, null); // 当前节点也存放了上一个节点的指针，指向前驱
    last = newNode;
    if (l == null)          // 对于第一次add元素，last = first
        first = newNode;
    else
        l.next = newNode;   // 否则，将尾部节点的指针指向新add的节点
    size++;
    modCount++;
}
```

## Vector

- 底层数据结构为数组
- 每次扩容为2倍进行
- 线程**安全**
- 默认初始大小为10

### 扩容策略(源码追踪)

```java
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    //此处有一个field为capacityIncrement，其用于设置扩容的增量。为0.1，则每次按照1.1进行递增扩容；构建vector时便可以预设
    int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                        capacityIncrement : oldCapacity); 
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    elementData = Arrays.copyOf(elementData, newCapacity);
}

public Vector(int initialCapacity, int capacityIncrement) {
    super();
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal Capacity: "+
                                            initialCapacity);
    this.elementData = new Object[initialCapacity];
    this.capacityIncrement = capacityIncrement; // 设置容器的每次扩容增量
}
```