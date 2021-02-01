### 题目描述

**`LRU`缓存机制：**

运用你所掌握的数据结构，设计和实现一个`LRU(最近最少使用)`缓存机制 。

实现 LRUCache 类：

- `LRUCache(int capacity)`以正整数作为容量`capacity`初始化`LRU`缓存
- `int get(int key)`如果关键字`key`存在于缓存中，则返回关键字的值，否则返回`-1`。
- `void put(int key, int value)`如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。
- 当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。

在 O(1) 时间复杂度内完成这两种操作。

**示例：**

```shell
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]
```

### 解法

**解法一：**

```go
type LinkNode struct {
	key       int
	val       int
	pre, next *LinkNode
}

type LRUCache struct {
	size, capacity int
	head, tail     *LinkNode
	m              map[int]*LinkNode
}


func Constructor(n int) LRUCache {
	size, capacity := n, 0
	head, tail := &LinkNode{}, &LinkNode{}
	head.next = tail
	tail.pre = head
	return LRUCache{size, capacity, head, tail, map[int]*LinkNode{}}
}


func (l *LRUCache) Get(k int) int {
	node := l.m[k]
	if node == nil {
		return -1
	}
	node.pre.next = node.next
	node.next.pre = node.pre

	node.next = l.head.next
	l.head.next = node
	node.next.pre = node
	node.pre = l.head
	return node.val
}


func (l *LRUCache) Put(k, v int) {
	if l.m[k] != nil {
		node := l.m[k]
		node.val = v
		node.pre.next = node.next
		node.next.pre = node.pre

		node.next, l.head.next = l.head.next, node
		node.next.pre, node.pre = node, l.head
	} else {
		node := &LinkNode{key: k, val: v}
		node.next, l.head.next = l.head.next, node
		node.next.pre, node.pre = node, l.head
		l.m[k] = node
		l.capacity++
	}
	if l.capacity > l.size {
		delete(l.m, l.tail.pre.key)
		l.tail.pre = l.tail.pre.pre
		l.tail.pre.next = l.tail
		l.capacity--
	}
	return
}


/**
 * Your LRUCache object will be instantiated and called as such:
 * obj := Constructor(capacity);
 * param_1 := obj.Get(key);
 * obj.Put(key,value);
 */
```