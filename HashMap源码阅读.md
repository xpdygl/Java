## 源码阅读



```java
public V put(K key, V value) {
    //调用 putVal 方法，传递了key的 hashcode、key、value
    return putVal(hash(key), key, value, false, true);
}
```



```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    //tab 记录底层数组
    //p 记录一个键值对
    //n 底层数组的长度
    //i 记录桶的位置
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    
    //判断底层数组是否为null 或者底层数组的长度是否为0
    //判断数组是否已经初始化
    if ((tab = table) == null || (n = tab.length) == 0)
        //调用 resize() 方法初始化底层数组
        n = (tab = resize()).length;
    
    
    //使用 key 的 hash 和数组的长度做取余操作，得到桶的位置
    //i =  hash % length
    //将桶中的元素取出来赋值给 p
    //判断桶中的元素是否为null
    if ((p = tab[i = (n - 1) & hash]) == null)
        //如果桶是空的，那么就将键值对包装到 Node 对象中，并直接放入桶中
        tab[i] = newNode(hash, key, value, null);
    else {
        //程序执行到这里，说明桶中有元素
        //声明了两个变量
        //e 记录冲突的键值对
        Node<K,V> e; K k;
        //判断桶中的第一个键值对的key是否和本次添加的key相同
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            //如果key相同，就将发生冲突的键值对赋值给e
            e = p;
        //判断当前桶中是否是一颗树接口
        else if (p instanceof TreeNode)
            //尝试将键值对信息添加到树结构中
            //添加成功：返回null
            //添加失败：返回冲突的节点
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            //程序走到这里，说明桶中是一个链表
            for (int binCount = 0; ; ++binCount) {
                //从链表的第二个元素开始遍历
                //如果当前遍历到的元素是null，说明已经遍历到链表的末尾
                if ((e = p.next) == null) {
                    //将待添加的键值对信息封装到Node对象中，并添加到链表的末尾
                    //尾插法
                    p.next = newNode(hash, key, value, null);
                    //元素添加到链表的末尾之后，判断链表是否需要树化
                    //链表的长度>8
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        //树化
                        treeifyBin(tab, hash);
                    break;
                }
                //在遍历链表的过程中，如果还没有到末尾，就需判断当前遍历到的键值对是否和待添加的键值对冲突
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    //如果冲突，结束链表的遍历
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            //记录旧的值
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                //值替换成新的值
                e.value = value;
            afterNodeAccess(e);
            //把旧的值返回
            return oldValue;
        }
    }
    //记录修改次数
    ++modCount;
    //判断当前元素的数量是否大于扩容阈值
    if (++size > threshold)
        resize(); //扩容
    afterNodeInsertion(evict);
    return null;
}
```



```java
final Node<K,V>[] resize() {
    //记录旧数组
    Node<K,V>[] oldTab = table;
    //记录旧数组的长度
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    //记录旧扩容阈值
    int oldThr = threshold;
    //声明了两个变量用于记录新的容量和新的扩容阈值
    int newCap, newThr = 0;
    //首次进入 resize 方法，这个判断不会成立
    if (oldCap > 0) {
        //如果旧容量已经大于或等于允许的最大容量，就不扩容
        if (oldCap >= MAXIMUM_CAPACITY) {
            //扩容阈值直接给到Integer最大值
            threshold = Integer.MAX_VALUE;
            return oldTab;
        }
        //新容量=旧容量*2
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
            //新阈值=旧阈值*2
            newThr = oldThr << 1; // double threshold
    }
    else if (oldThr > 0) //首次进入 resize 方法，这个判断不会成立
        newCap = oldThr;
    else {               // zero initial threshold signifies using defaults
        //默认的容量是16
        newCap = DEFAULT_INITIAL_CAPACITY;
        //扩容阈值是 负载因子 * 默认容量 = 12
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
    }
    //将计算出来的扩容阈值赋值给 threshold
    threshold = newThr;
    @SuppressWarnings({"rawtypes","unchecked"})
    //创建新数组，newCap=16
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
    //将新数组赋值给 table
    table = newTab;
    //如果旧数组不是null，那么就要将旧数组中的数据迁移到新数组中
    if (oldTab != null) {
        //将所有元素重新计算桶的位置，并将数据放入新数组中
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
            if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
                if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
                else if (e instanceof TreeNode)
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
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
            }
        }
    }
    return newTab;
}
```



```java
final void treeifyBin(Node<K,V>[] tab, int hash) {
    int n, index; Node<K,V> e;
    //如果当前数组的长度小于64，则不进行树化，而是直接扩容
    if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
        resize();
    else if ((e = tab[index = (n - 1) & hash]) != null) {
        TreeNode<K,V> hd = null, tl = null;
        do {
            TreeNode<K,V> p = replacementTreeNode(e, null);
            if (tl == null)
                hd = p;
            else {
                p.prev = tl;
                tl.next = p;
            }
            tl = p;
        } while ((e = e.next) != null);
        if ((tab[index] = hd) != null)
            hd.treeify(tab);
    }
}
```



## 结论



HashMap 基于数组+链表+红黑树



初始容量：16

负载因子：0.75

扩容阈值：容量*负载因子



扩容：每次扩容新容量都是旧容量的2倍。



1、根据key的hashcode计算桶的位置

2、如果桶中是空的，直接插入键值对

3、判断桶中的第一个元素是否冲突

4、如果桶中是一颗树结构，尝试将键值对插入到树中

5、如果桶中是一个链表结果，遍历链表中的所有键值对

6、在链表中添加完元素之后，需要判断链表长度是否大于树化阈值，如果大于，并且数组长度达到64，才进行树化

7、如果有冲突，替换值，返回旧的值

8、没有冲突，判断是否需要扩容