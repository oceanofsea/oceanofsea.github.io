---
layout: post
title:  "HashMap源码分析"
date:   2016-02-28
categories: Java
excerpt: 
tags: Java HashMap
---

* content
{:toc}

Java HashMap 实现了Map接口, permits null key nad null values, it does not guarantee that the order will remain constant over time.

整体实现是数组加链表（当链表太长时，为了加快查找，将链表转成TreeNode）
 
## 影响HashMap的性能的两个因素
 
 - initial capacity (capacity is the number of buckets in the hash table)
 	- In general set as 0.75
 - load factor (how full is the hash table is allowed to get)
 
## Important things when using HashMap
- 预估初始的容量大小很重要
	- When the number of entries in the hash table exceeds the product of the 	load factor and the current capacity, the hash table is **rehashed** (that 	is, internal data structures are rebuilt) so that the hash table has 	approximately twice the number of buckets.
	- Iteration over collection views requires time proportional to the 	"capacity" of the HashMap instance (the number of buckets) plus its size 	(the number
	 of key-value mappings).  **Thus, it's very important not to set the initial
	 capacity too high (or the load factor too low) if iteration performance is
	 important.**
- Note that this implementation is not synchronized.
 	- If multiple threads access a hash map concurrently, and at least one of
 the threads modifies the map structurally, it must be
 synchronized externally.
 
## Implementation

 This map usually acts as a binned (bucketed) hash table, but
     when bins get too large, they are transformed into bins of
     TreeNodes, each structured similarly to those in
     java.util.TreeMap. Most methods try to use normal bins, but
      relay to TreeNode methods when applicable (simply by checking
      instanceof a node) for faster lookup.
### Hash Function

```
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

Computes key.hashCode() and spreads (XORs) higher bits of hash
to lower.  Because the table uses power-of-two masking, sets of
hashes that vary only in bits above the current mask will
always collide. (Among known examples are sets of Float keys
holding consecutive whole numbers in small tables.)  So we
apply a transform that spreads the impact of higher bits
downward.

### Fields

```
transient Node<K,V>[] table;

/**
 * Holds cached entrySet(). Note that AbstractMap fields are used
 * for keySet() and values().
 */
transient Set<Map.Entry<K,V>> entrySet;

transient int size;

/**
 * The number of times this HashMap has been structurally modified
 */
transient int modCount;

/**
 * The next size value at which to resize (capacity * load factor).
 */
int threshold;

/**
 * The load factor for the hash table.
 */
final float loadFactor;
```

### Public operations

#### Construction
构造函数：默认的initial capacity=16, load factor=0.75

从已有Map构建hashmap

```
/**
 * Constructs a new <tt>HashMap</tt> with the same mappings as the
 * specified <tt>Map</tt>.  The <tt>HashMap</tt> is created with
 * default load factor (0.75) and an initial capacity sufficient to
 * hold the mappings in the specified <tt>Map</tt>.
 *
 * @param   m the map whose mappings are to be placed in this map
 * @throws  NullPointerException if the specified map is null
 */
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false);
}

/**
 * Implements Map.putAll and Map constructor
 *
 * @param m the map
 * @param evict false when initially constructing this map, else
 * true (relayed to method afterNodeInsertion).
 */
final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
    int s = m.size();
    if (s > 0) {
        if (table == null) { // pre-size
            float ft = ((float)s / loadFactor) + 1.0F;
            int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                     (int)ft : MAXIMUM_CAPACITY);
            if (t > threshold)
                threshold = tableSizeFor(t);
        }
        else if (s > threshold)
            resize();
        for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
            K key = e.getKey();
            V value = e.getValue();
            putVal(hash(key), key, value, false, evict);
        }
    }
}
```

#### get

```
	public V get(Object key) {
	    Node<K,V> e;
	    return (e = getNode(hash(key), key)) == null ? null : e.value;
	}

    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
```
get的时候总是先查看第一个元素，如果不是我们要的value，则继续查找，如果此时是treeNode的形式，就按treeNode的方式获取，否则就顺序检查直到找到想要的元素。

**非常重要的是：**当你返回的元素是null时，不代表这个key不存在！因为HashMap是允许null值的！
用containsKey可以区分这种情况。

```
public boolean containsKey(Object key) {
    return getNode(hash(key), key) != null;
}
```

#### put

put的时候，先检查这个bucket是不是空，如果为空就新建一个节点。
否则检查这个key是不是存在，如果存在，则替换原来的值，并返回旧的值
如果key不存在，则看结构是tree还是普通list进行插入，并根据binCount决定是否要转换成tree
最后检查size来决定是否要resize

```
  public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    /**
     * Implements Map.put and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        // bucket为空
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                    	// 将value链接到已存在的linkedlist上
                        p.next = newNode(hash, key, value, null);
                        // 如果bucket里的数太多，就将它转成tree的结构
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

#### remove
先找到要删除的node，然后进行删除

```
    public V remove(Object key) {
        Node<K,V> e;
        return (e = removeNode(hash(key), key, null, false, true)) == null ?
            null : e.value;
    }

    /**
     * Implements Map.remove and related methods
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to match if matchValue, else ignored
     * @param matchValue if true only remove if value is equal
     * @param movable if false do not move other nodes while removing
     * @return the node, or null if none
     */
    final Node<K,V> removeNode(int hash, Object key, Object value,
                               boolean matchValue, boolean movable) {
        Node<K,V>[] tab; Node<K,V> p; int n, index;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (p = tab[index = (n - 1) & hash]) != null) {
            Node<K,V> node = null, e; K k; V v;
            // 找到要删除的node
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                node = p;
            else if ((e = p.next) != null) {
                if (p instanceof TreeNode)
                    node = ((TreeNode<K,V>)p).getTreeNode(hash, key);
                else {
                    do {
                        if (e.hash == hash &&
                            ((k = e.key) == key ||
                             (key != null && key.equals(k)))) {
                            node = e;
                            break;
                        }
                        p = e;
                    } while ((e = e.next) != null);
                }
            }
            // 删除找到的node
            if (node != null && (!matchValue || (v = node.value) == value ||
                                 (value != null && value.equals(v)))) {
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
            }
        }
        return null;
    }
```