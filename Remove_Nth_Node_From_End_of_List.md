### 题目描述

**删除链表的倒数第`N`个节点：**

删除给定链表的倒数第`N`个节点，返回链表头节点。

**示例：**

```shell
输入：head = [1, 2, 3, 4, 5], n = 2
输出：[1, 2, 3, 5]
```

### 解法

`go`的链表节点定义为：

```go
type ListNode struct {
	Val  int
	Next *ListNode
}
```

**解法一：两次遍历**

先求出链表长度，再删除指定链表。代码略。

**解法二：数组或栈**

将链表节点放入数组，然后根据根据下标删除指定节点。代码略。

**解法三：快慢指针**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func removeNthFromEnd(head *ListNode, n int) *ListNode {
	dummy := &ListNode{Next: head}
	slow, fast := dummy, dummy
	// 让快慢指针相差n个节点
	counts := 0
	for counts < n && fast != nil {
		fast = fast.Next
		counts++
	}

	if fast == nil {
		return dummy.Next
	}

	for fast.Next != nil {
		slow = slow.Next
		fast = fast.Next
	}

	slow.Next = slow.Next.Next
	return dummy.Next
}
```