设计循环双端队列

----

题目描述

设计实现双端队列，需要支持以下操作：

- `MyCircularDeque(k)`：构造函数,双端队列的大小为k。
- `insertFront()`：将一个元素添加到双端队列头部。 如果操作成功返回 true。
- `insertLast()`：将一个元素添加到双端队列尾部。如果操作成功返回 true。
- `deleteFront()`：从双端队列头部删除一个元素。 如果操作成功返回 true。
- `deleteLast()`：从双端队列尾部删除一个元素。如果操作成功返回 true。
- `getFront()`：从双端队列头部获得一个元素。如果双端队列为空，返回 -1。
- `getRear()`：获得双端队列的最后一个元素。 如果双端队列为空，返回 -1。
- `isEmpty()`：检查双端队列是否为空。
- `isFull()`：检查双端队列是否满了。

示例：

```shell
MyCircularDeque circularDeque = new MycircularDeque(3); // 设置容量大小为3
circularDeque.insertLast(1);			                // 返回 true
circularDeque.insertLast(2);			                // 返回 true
circularDeque.insertFront(3);			                // 返回 true
circularDeque.insertFront(4);			                // 已经满了，返回 false
circularDeque.getRear();  				                // 返回 2
circularDeque.isFull();				                    // 返回 true
circularDeque.deleteLast();			                    // 返回 true
circularDeque.insertFront(4);			                // 返回 true
circularDeque.getFront();				                // 返回 4
```

----

### 解法

解法一：数组实现

- 时间复杂度：`O(1)`
- 空间复杂度：`O(1)`

```go
type MyCircularDeque struct {
	nums   []int
	counts int
}

/** Initialize your data structure here. Set the size of the deque to be k. */
func Constructor(k int) MyCircularDeque {
	return MyCircularDeque{
		nums:   []int{},
		counts: k,
	}
}

/** Adds an item at the front of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) InsertFront(value int) bool {
	if len(this.nums) >= this.counts {
		return false
	}

	this.nums = append([]int{value}, this.nums...)
	return true
}

/** Adds an item at the rear of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) InsertLast(value int) bool {
	if len(this.nums) >= this.counts {
		return false
	}
	this.nums = append(this.nums, value)
	return true
}

/** Deletes an item from the front of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) DeleteFront() bool {
	if len(this.nums) <= 0 {
		return false
	}
	this.nums = this.nums[1:]
	return true
}

/** Deletes an item from the rear of Deque. Return true if the operation is successful. */
func (this *MyCircularDeque) DeleteLast() bool {
	if len(this.nums) <= 0 {
		return false
	}
	this.nums = this.nums[:len(this.nums)-1]
	return true
}

/** Get the front item from the deque. */
func (this *MyCircularDeque) GetFront() int {
	if len(this.nums) <= 0 {
		return -1
	}
	num := this.nums[0]
	return num
}

/** Get the last item from the deque. */
func (this *MyCircularDeque) GetRear() int {
	if len(this.nums) <= 0 {
		return -1
	}
	num := this.nums[len(this.nums)-1]
	return num
}

/** Checks whether the circular deque is empty or not. */
func (this *MyCircularDeque) IsEmpty() bool {
	return len(this.nums) <= 0
}

/** Checks whether the circular deque is full or not. */
func (this *MyCircularDeque) IsFull() bool {
	return len(this.nums) >= this.counts
}
```

解法二：链表实现

代码略

