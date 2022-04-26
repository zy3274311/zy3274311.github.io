# Java Develop

## JVM

## 内存管理

## 多线程与线程锁
* 线程池
* 线程锁

## 泛型<>
* super关键字在泛型中的使用
* extend关键字在泛型中的使用
* ?的使用
* 能否将固定格式强转位泛型格式

## 序列化
* Serializable
* Externalizable
* Parcelable

## JNI
* 内存管理
* 静态注册、动态注册

## java中的数据结构
### HashMap
* 初始化加载因子loadFactor（默认0.75f）
* 初始容量initialCapacity，通过无符号右移计算出大于且最接近initialCapacity的threshold临界值为``$2^n$``
```
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```
* hash计算，hashcode前16位与后16位进行异或位运算
```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
* HashMap内的数据存储在table数组中，table的元素为Node链表结构，当Node链表长度>=8时转换为红黑树，同时Node转换为TreeNode
* put操作
    * table为空则resize进行扩容
    ```
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    ```
    * 计算key对应在table中的index位置，若当前index无数据则直接赋值新Node
    ```
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    ```
    * 若当前index存在数据
        * 若当前Node的Key是否与插入Key一致，则插入Value
        ```
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        ```
        * 若当前Node为TreeNode则插入红黑树
        ```
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        ```
        * 否则插入到链表Node中
            * 若在Node链表中查找到插入Key则插入Value
            * 否则创建直接在Node链表的末尾插入新Node，插入后若链表长度是否达到临界值8，则将链表转换为红黑树
        ```
        for (int binCount = 0; ; ++binCount) {
            if ((e = p.next) == null) {
                p.next = newNode(hash, key, value, null);
                if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                    treeifyBin(tab, hash);
                break;
            }
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                break;
            p = e;
        }
        ```
        * 插入存在的Key的Node后需要调用afterNodeAccess通知LinkedHashMap
        ```
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
        ```
    * 完成put操作后若HashMap长度大于threshold临界值，则进行resize扩容，最后调用afterNodeInsertion通知LinkedHashMap
    ```
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    ```
* resize扩容
    * 初次扩容，使用默认值，如已经有threshold,则直接使用旧threshold左右新的容量，新threshold临界值为新容量乘以加载因子capacity*loadFactor
        ```
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        ```
    * 当达到容量最大值时不再扩容，直接返回旧table
        ```
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        ```
    * 扩容容量新capacity为旧capacity的2倍
        ```
        if (oldCap >= MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        ```
    * 旧table中的元素转移到新table中
        * 当table中元素只有一个，则直接移动
            ```
            if (e.next == null)
                newTab[e.hash & (newCap - 1)] = e;
            ```
        * 当table中的元素为TreeNode结构时，调用TreeNode的split方法，将树结构分为高低两个链表操作类似链表Node移动，分割链表元素书<=6则取消TreeNode改为Node，否则再次树化分割后的链表
            ```
            else if (e instanceof TreeNode)
                ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
            ```
        * 链表Node移动，分为高低两个链表，低链表仍然在原来的index，高链表节点放到index+oldCap的位置
            ```
            else { // preserve order
                Node<K,V> loHead = null, loTail = null;
                Node<K,V> hiHead = null, hiTail = null;
                Node<K,V> next;
                do {
                    next = e.next;
                    if ((e.hash & oldCap) == 0) {
                        if (loTail == null)
                            loHead = e;
                        else
                            loTail.next = e;
                        loTail = e;
                    }
                    else {
                        if (hiTail == null)
                            hiHead = e;
                        else
                            hiTail.next = e;
                        hiTail = e;
                    }
                } while ((e = next) != null);
                if (loTail != null) {
                    loTail.next = null;
                    newTab[j] = loHead;
                }
                if (hiTail != null) {
                    hiTail.next = null;
                    newTab[j + oldCap] = hiHead;
                }
            }
            ```
* remove操作
    * 根据Key查找当前删除的Node
    * 若Node是TreeNode，则调用删除树节点方法
    * 若Node节点是Table元素，则把Node的next赋值给Table元素
    * 否则将Node按照链表删除方式删除，即把前一个Node的next指向当前节点的next
    ```
    if (node instanceof TreeNode)
        ((TreeNode<K,V>)node).removeTreeNode(this, tab, movable);
    else if (node == p)
        tab[index] = node.next;
    else
        p.next = node.next;
    ++modCount;
    --size;
    afterNodeRemoval(node);
    return node;
    ```
* 链表树化treeifyBin，链表Node转换为TreeNode红黑树结构
* modCount作用，安全检查，防止遍历HashMap元素时，删增HashMap
* 红黑树
    
### ConcurrentHashMap
* 分段锁
    
### LinkedList
### TreeMap

## 动态代理

## 反射

## 动态加载

## 特殊关键字
transient 防止Serializable序列化的字段
default 