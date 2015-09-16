---
title: java-hashmap
date: 2015-09-16 12:47:54
categories: java
tags: hashmap
---
1. HashMap 概述：
   HashMap 是基于哈希表的 Map 接口的非同步实现。此实现提供所有可选的映射操作，并允许使用 null 值和 null 键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。
 
2. HashMap 的数据结构：
   - 在 java 编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，HashMap 也不例外。HashMap 实际上是一个“链表散列”的数据结构，即数组和链表的结合体 (邻接表?)。
   - HashMap 底层就是一个数组结构，数组中的每一项又是一个链表。当新建一个 HashMap 的时候，就会初始化一个数组。

    ```java
    /** 
     * The table, resized as necessary. Length MUST Always be a power of two. 
     */  
    transient Entry[] table;  
      
    static class Entry<K,V> implements Map.Entry<K,V> {  
        final K key;  
        V value;  
        Entry<K,V> next;  
        final int hash;  
        ……  
    } 
    ```
 可以看出，Entry 就是数组中的元素，每个 Map.Entry 其实就是一个 key-value 对，它持有一个指向下一个元素的引用，这就构成了链表。

3. HashMap 的存取实现：

    ```
    
    public V put(K key, V value) {
        // HashMap 允许存放 null 键和 null 值。
        // 当 key 为 null 时，调用 putForNullKey 方法，将 value 放置在数组第一个位置。
        if (key == null)
            return putForNullKey(value);
        // 根据 key 的 keyCode 重新计算 hash 值。
        int hash = hash(key.hashCode());
        // 搜索指定 hash 值在对应 table 中的索引。
        int i = indexFor(hash, table.length);
        // 如果 i 索引处的 Entry 不为 null，通过循环不断遍历 e 元素的下一个元素。
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            // 如果 hash 相等且 entry 上的 key 与新 key 相等，那么替换掉原 value。否则为添加 entry 到链表
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
        // 如果 i 索引处的 Entry 为 null，表明此处还没有 Entry。
        modCount++;
        // 将 key、value 添加到 i 索引处。
        addEntry(hash, key, value, i);
        return null;
    }
    
    ```
