合并两个有序链表

----

### 题目描述

> 将两个升序列表合并为一个升序列表，并且返回

示例：

```shell
输入：1 -> 2 -> 4, 1 -> 3 -> 4
输出：1 -> 1 -> 2 -> 3 -> 4 -> 4
```

----

### 解法

方法一：递归

- 时间复杂度：O(n + m), n 和 m 是链表长度
- 空间复杂度：O(n + m), n 和 m 是链表长度

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	if l1 == nil {
		return l2
	}
	if l2 == nil {
		return l1
	}
	if l1.Val < l2.Val {
		l1.Next = mergeTwoLists(l1.Next, l2)
		return l1
	} else {
		l2.Next = mergeTwoLists(l2.Next, l1)
		return l2
	}
}
```

方法二：迭代

- 时间复杂度：O(n + m), n 和 m 是链表长度
- 空间复杂度：O(1)

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	dummy := &ListNode{}
	tmp := dummy
	for l1 != nil && l2 != nil {
		if l1.Val < l2.Val {
			dummy.Next = l1
			l1 = l1.Next
		} else {
			dummy.Next = l2
			l2 = l2.Next
		}
		tmp = tmp.Next
	}

	if l1 == nil {
		tmp.Next = l2
	} else if l2 == nil {
		tmp.Next = l1
	}

	return dummy.Next
}
```