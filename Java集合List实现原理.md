# 一、集合类结构

顶层类接口 Iterable：只有一个方法

```
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

次顶层类接口 Collection：继承自Iterable,新增 add、clear、contains、remove、size、isEmpty等方法

```
public interface Collection<E> extends Iterable<E> {
    boolean add(E var1);

    boolean addAll(Collection<? extends E> var1);

    void clear();

    boolean contains(Object var1);

    boolean containsAll(Collection<?> var1);

    boolean equals(Object var1);

    int hashCode();

    boolean isEmpty();

    Iterator<E> iterator();

    boolean remove(Object var1);

    boolean removeAll(Collection<?> var1);

    boolean retainAll(Collection<?> var1);

    int size();

    Object[] toArray();

    <T> T[] toArray(T[] var1);
}
```

AbstractCollection：实现了Collection的抽象类，注意是抽象类

List:继承了Collection接口。 新增get方法，indexOf方法，lastIndexOf方法，set方法,subList方法

Queue：继承了Collection接口。新增offer、poll、element、peek方法。

Set：继承了Collection接口。没有新增方法。

AbstractList:继承自AbstractCollection，并实现List接口，仍是抽象类。这里会有部分方法重合，毕竟AbstractCollection和List一个实现一个继承了Collection接口。然后新增removeRange方法

### ArrayList

继承自AbstractList，并实现Cloneable, Serializable, RandomAccess接口。
底层实现是数组，默认初始化一个长度为10的数组，也可指定大小。后续新增超标的数据时，需要扩容。扩容机制是，新增当前长度大小的一半。

jdk1.8的扩容算法：newCapacity = oldCapacity + ( oldCapacity >> 1 ) ;   // oldCapacity >> 2  移位运算，此处相当于oldCapacity除以2，但是 >> 这种写法更加高效

jdk1.6的扩容算法：newCapacity = ( oldCapacity * 3 ) / 2 +1 ;

参数介绍：newCapacity 是扩容后的容量大小，oldCapacity 是扩容前的大小

### Vector

继承自AbstractList，并实现Cloneable, Serializable, RandomAccess接口。
底层同样是数组实现，默认长度也是10。与ArrayList不同的是，可以指定扩容的新增长度。如果不指定就默认新增当前长度大小的一倍，旧版是两倍。

jdk1.8的扩容算法：newCapacity = oldCapacity + ( ( capacityIncrement > 0 ) ? capacityIncrement : oldCapacity );

jdk1.6的扩容算法：newCapacity = ( capacityIncrement > 0 ) ? ( oldCapacity + capacityIncrement ) : (  oldCapacity  * 2 );

参数介绍：capacityIncrement 是容量修正（即容量新增大小），没有设置，默认为0    ，newCapacity 是扩容后的容量大小，oldCapacity 是扩容前的大小

### LinkedList

Deque：继承自Queue接口

```
public interface Deque<E> extends Queue<E> {
    void addFirst(E var1);

    void addLast(E var1);

    boolean offerFirst(E var1);

    boolean offerLast(E var1);

    E removeFirst();

    E removeLast();

    E pollFirst();

    E pollLast();

    E getFirst();

    E getLast();

    E peekFirst();

    E peekLast();

    boolean removeFirstOccurrence(Object var1);

    boolean removeLastOccurrence(Object var1);

    boolean add(E var1);

    boolean offer(E var1);

    E remove();

    E poll();

    E element();

    E peek();

    void push(E var1);

    E pop();

    boolean remove(Object var1);

    boolean contains(Object var1);

    int size();

    Iterator<E> iterator();

    Iterator<E> descendingIterator();
}
```

AbstractSequentialList：继承自AbstractList的抽象类。没有新增方法

LinkedList：继承自AbstractSequentialList，实现List<E>, Deque<E>, Queue<E>, Cloneable, Serializable接口。底层实现是双向链表，所以没有容量大小定义。只有上个节点、当前节点、下个节点。每一个节点都有上个节点和下个节点。

- 头节点新增
先把上次的头节点赋给临时节点，借临时节点创建新的当前节点，把临时节点赋予新当前节点的下个节点，把临时节点的上个节点指向新的当前节点，最后更新链表的保存的头节点数据，指向新的当前节点。
为空代表链表没有元素
```
private void linkFirst(E e) {
        final Node<E> f = first;
        final Node<E> newNode = new Node<>(null, e, f);
        first = newNode;
        if (f == null)
            last = newNode;
        else
            f.prev = newNode;
        size++;
        modCount++;
    }
```

- 尾节点新增

先把上次的尾节点赋给临时节点，借临时节点创建新的当前节点，把临时节点赋予新当前节点的上个节点，把临时节点的下个节点指向新的当前节点，最后更新链表的保存的尾节点数据，指向新的当前节点。
为空代表链表没有元素

```
void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }
```

- 删节点
把当前节点赋给临时变量节点，当前节点的上个节点，当前节点的下个节点都赋予临时变量。把临时的上个节点的下个节点指向临时的下个节点，
再把临时的下个节点的上个节点指向临时的上个节点，最后把临时当前节点的上下节点和自身置空。

```
E unlink(Node<E> x) {
        // assert x != null;
        final E element = x.item;
        final Node<E> next = x.next;
        final Node<E> prev = x.prev;

        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }

        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }

        x.item = null;
        size--;
        modCount++;
        return element;
    }
```

