---
layout:   page
title: 如何设计一个hashmap
tag: devops
category: ood
order: 1
author:      "aqiu"
image: ''
published: true
categories: ['OOD']
date: 2024-01-01
---
- ## HashMap的特性是什么
    - 基本用过`hashmap`的都会了解到，一个`hashmap`具备以下特性
        - 快速查找： `O(1)` 时间复杂的查找效率
          logseq.order-list-type:: number
        - 无需排序： `hashmap`中的元素不存在顺序
          logseq.order-list-type:: number
        - 额外开销：为了存储更多信息，可能会带来一些额外的开销
          logseq.order-list-type:: number
- ## 设计HashMap需要考虑哪些问题
    - ### Hash Function
        - #### 什么是hash function
            - `hash function` (散列函数) 的作用是将输入（通常是字符串或数值）映射到固定大小的数值的函数。这个输出数值被称为散列值（Hash Value）或散列码（Hash Code）
            - `例子`：
              -
                ```python
                hash(x) = x mod M
                ```
        - #### 常见的hash function
            - sha-256
            - md5
            - sha-1
    - ### Hash碰撞
        - 如果我们的`hash function` 将两个不同的`key` 映射到了相同的`value`, 这就是所谓的哈希碰撞
            - ![](https://golearning.oss-cn-shanghai.aliyuncs.com/obsidian20240102142008.png)
        - 如何解决哈希碰撞
            - ##### 链地址法（Separate Chaining）
              collapsed:: true
                - **原理**：在哈希表的每个槽位上维护一个链表。当碰撞发生时，简单地将具有相同哈希值的元素添加到对应槽位的链表中。
                - **优点**：实现简单，链表可以无限增长。
                - **缺点**：链表较长时，查找效率可能下降。
            - ##### 开放寻址法（Open Addressing）
              collapsed:: true
                - **原理**：当碰撞发生时，在哈希表中寻找另一个空闲槽位来存储元素。常见的策略包括线性探测、二次探测和双重散列。
                    - **线性探测**：顺序查找下一个空闲槽位。
                    - **二次探测**：使用二次函数来决定查找的步长。
                    - **双重散列**：使用第二个哈希函数确定查找步长。
                - **优点**：所有元素都直接存储在哈希表中，不需要额外空间。
                - **缺点**：可能出现“聚集”现象，影响性能。
            - ##### 再散列（Rehashing）
              collapsed:: true
                - **原理**：当哈希表达到一定的填充程度（即装载因子过高），通过创建一个更大的哈希表，并使用新的哈希函数（或同一哈希函数但不同的大小参数），将所有元素重新散列到新表中。
                - **优点**：可以显著减少碰撞，提高操作效率。
                - **缺点**：再散列过程可能比较耗时。
            - ##### 使用更好的哈希函数
              collapsed:: true
                - **原理**：选择或设计一个更加均匀分布输出的哈希函数，以减少碰撞的概率。
                - **优点**：直接减少碰撞发生的频率。
                - **缺点**：对于某些数据集，可能难以找到完美的哈希函数。
            - ##### 一致性哈希（Consistent Hashing）
              collapsed:: true
                - **原理**：特别适用于分布式系统，通过一致性哈希算法将数据均匀分布在不同的节点上。
                - **优点**：在节点增加或减少时，只影响邻近区域的数据分布，降低重分配的成本。
                - **缺点**：实现相对复杂，主要用于分布式系统。
    - ### Buckets
        - `buckets` 就是专门用于存储 `hashmap`的部分
    - ### Capacity 和 Load Factor
        - `Capacity`一般指的是`bucket`能存储的数据的数量
        - `Load Factor`负载因子是衡量哈希表满的程度的一个指标。它是哈希表中已经存储的元素数量与容量的比值。负载因子 = 元素数量 / 容量。负载因子用于判断何时需要调整哈希表的容量。通常，负载因子设定一个阈值，比如0.75。当哈希表中的元素数量超过了容量与这个阈值的乘积时，哈希表需要进行扩容（rehashing），即增加容量并重新分布现有的元素。
- ## 具体设计
  -
    ```golang
    package main
    
    import (
        "fmt"
        "hash/fnv"
    )
    
    type HashMap struct {
        buckets [][]KeyValue
        size    int
        count   int
    }
    
    type KeyValue struct {
        key   string
        value interface{}
    }
    
    func NewHashMap(size int) *HashMap {
        return &HashMap{
            buckets: make([][]KeyValue, size),
            size:    size,
        }
    }
    
    func (h *HashMap) hashFunction(key string) int {
        hf := fnv.New32a()
        hf.Write([]byte(key))
        return int(hf.Sum32()) % h.size
    }
    
    func (h *HashMap) Insert(key string, value interface{}) {
        if float64(h.count)/float64(h.size) >= 0.75 {
            h.resize()
        }
    
        index := h.hashFunction(key)
        for i, kv := range h.buckets[index] {
            if kv.key == key {
                h.buckets[index][i].value = value
                return
            }
        }
    
        h.buckets[index] = append(h.buckets[index], KeyValue{key, value})
        h.count++
    }
    
    func (h *HashMap) Get(key string) (interface{}, bool) {
        index := h.hashFunction(key)
        for _, kv := range h.buckets[index] {
            if kv.key == key {
                return kv.value, true
            }
        }
        return nil, false
    }
    
    func (h *HashMap) resize() {
        newSize := h.size * 2
        newBuckets := make([][]KeyValue, newSize)
        oldBuckets := h.buckets
        h.buckets = newBuckets
        h.size = newSize
        h.count = 0
    
        for _, bucket := range oldBuckets {
            for _, kv := range bucket {
                h.Insert(kv.key, kv.value)
            }
        }
    }
    
    func main() {
        hm := NewHashMap(16)
        hm.Insert("key1", "value1")
        hm.Insert("key2", 100)
        hm.Insert("key3", true)
    
        if value, found := hm.Get("key2"); found {
            fmt.Printf("Found key2 with value %v\n", value)
        }
    }
    
    ```