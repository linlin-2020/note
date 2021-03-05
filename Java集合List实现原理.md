# һ��������ṹ

������ӿ� Iterable��ֻ��һ������

```
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

�ζ�����ӿ� Collection���̳���Iterable,���� add��clear��contains��remove��size��isEmpty�ȷ���

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

AbstractCollection��ʵ����Collection�ĳ����࣬ע���ǳ�����

List:�̳���Collection�ӿڡ� ����get������indexOf������lastIndexOf������set����,subList����

Queue���̳���Collection�ӿڡ�����offer��poll��element��peek������

Set���̳���Collection�ӿڡ�û������������

AbstractList:�̳���AbstractCollection����ʵ��List�ӿڣ����ǳ����ࡣ������в��ַ����غϣ��Ͼ�AbstractCollection��Listһ��ʵ��һ���̳���Collection�ӿڡ�Ȼ������removeRange����

### ArrayList

�̳���AbstractList����ʵ��Cloneable, Serializable, RandomAccess�ӿڡ�
�ײ�ʵ�������飬Ĭ�ϳ�ʼ��һ������Ϊ10�����飬Ҳ��ָ����С�������������������ʱ����Ҫ���ݡ����ݻ����ǣ�������ǰ���ȴ�С��һ�롣

jdk1.8�������㷨��newCapacity = oldCapacity + ( oldCapacity >> 1 ) ;   // oldCapacity >> 2  ��λ���㣬�˴��൱��oldCapacity����2������ >> ����д�����Ӹ�Ч

jdk1.6�������㷨��newCapacity = ( oldCapacity * 3 ) / 2 +1 ;

�������ܣ�newCapacity �����ݺ��������С��oldCapacity ������ǰ�Ĵ�С

### Vector

�̳���AbstractList����ʵ��Cloneable, Serializable, RandomAccess�ӿڡ�
�ײ�ͬ��������ʵ�֣�Ĭ�ϳ���Ҳ��10����ArrayList��ͬ���ǣ�����ָ�����ݵ��������ȡ������ָ����Ĭ��������ǰ���ȴ�С��һ�����ɰ���������

jdk1.8�������㷨��newCapacity = oldCapacity + ( ( capacityIncrement > 0 ) ? capacityIncrement : oldCapacity );

jdk1.6�������㷨��newCapacity = ( capacityIncrement > 0 ) ? ( oldCapacity + capacityIncrement ) : (  oldCapacity  * 2 );

�������ܣ�capacityIncrement ������������������������С����û�����ã�Ĭ��Ϊ0    ��newCapacity �����ݺ��������С��oldCapacity ������ǰ�Ĵ�С

### LinkedList

Deque���̳���Queue�ӿ�

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

AbstractSequentialList���̳���AbstractList�ĳ����ࡣû����������

LinkedList���̳���AbstractSequentialList��ʵ��List<E>, Deque<E>, Queue<E>, Cloneable, Serializable�ӿڡ��ײ�ʵ����˫����������û��������С���塣ֻ���ϸ��ڵ㡢��ǰ�ڵ㡢�¸��ڵ㡣ÿһ���ڵ㶼���ϸ��ڵ���¸��ڵ㡣

- ͷ�ڵ�����
�Ȱ��ϴε�ͷ�ڵ㸳����ʱ�ڵ㣬����ʱ�ڵ㴴���µĵ�ǰ�ڵ㣬����ʱ�ڵ㸳���µ�ǰ�ڵ���¸��ڵ㣬����ʱ�ڵ���ϸ��ڵ�ָ���µĵ�ǰ�ڵ㣬����������ı����ͷ�ڵ����ݣ�ָ���µĵ�ǰ�ڵ㡣
Ϊ�մ�������û��Ԫ��
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

- β�ڵ�����

�Ȱ��ϴε�β�ڵ㸳����ʱ�ڵ㣬����ʱ�ڵ㴴���µĵ�ǰ�ڵ㣬����ʱ�ڵ㸳���µ�ǰ�ڵ���ϸ��ڵ㣬����ʱ�ڵ���¸��ڵ�ָ���µĵ�ǰ�ڵ㣬����������ı����β�ڵ����ݣ�ָ���µĵ�ǰ�ڵ㡣
Ϊ�մ�������û��Ԫ��

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

- ɾ�ڵ�
�ѵ�ǰ�ڵ㸳����ʱ�����ڵ㣬��ǰ�ڵ���ϸ��ڵ㣬��ǰ�ڵ���¸��ڵ㶼������ʱ����������ʱ���ϸ��ڵ���¸��ڵ�ָ����ʱ���¸��ڵ㣬
�ٰ���ʱ���¸��ڵ���ϸ��ڵ�ָ����ʱ���ϸ��ڵ㣬������ʱ��ǰ�ڵ�����½ڵ�������ÿա�

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

