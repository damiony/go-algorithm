### 题目描述

**相交链表：**

找到两个单链表相交的的起始节点。

### 解法

**链表节点为：**

```go
type ListNode struct {
  Val int
  Next *ListNode
}
```

**解法一：暴力**

对于链表`A`的每个节点，在链表`B`中查找是否有重复节点。

**解法二：哈希**

遍历链表`A`的每个节点并且存储，然后遍历链表`B`，找到重复节点。

**解法三：双指针**

一个指针指向`A`，一个指针指向`B`，同时向前走。走到结尾再从另一个链表开始，直到相遇。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
	if headA == nil || headB == nil {
		return nil
	}
	node1, node2 := headA, headB
	for node1 != node2 {
		if node1 == nil {
			node1 = headB
		} else {
			node1 = node1.Next
		}
		if node2 == nil {
			node2 = headA
		} else {
			node2 = node2.Next
		}
	}
	return node1
}
```