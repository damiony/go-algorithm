### 题目描述

**`K`个一组翻转链表：**

给定一个链表，每`K`个节点进行翻转，返回翻转后的链表。
`K`是正整数，小于等于链表的长度
如果节点总数不是`K`的整数倍，则保持原有顺序

**示例：**

```shell
输入：1 -> 2 -> 3 -> 4 -> 5
输出：
当 K = 2, 返回 2 -> 1 -> 4 -> 3 -> 5
当 K = 3, 返回 3 -> 2 -> 1 -> 4 -> 5
```

### 解法

**方法一：递归**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func reverseKGroup(head *ListNode, k int) *ListNode {
	if head == nil {
		return head
	}

	counts := 1
	tail := head
	for counts < k && tail != nil {
		tail = tail.Next
		counts++
	}

	if tail == nil {
		return head
	}

	tail.Next = reverseKGroup(tail.Next, k)
	for head != tail {
		tmp := tail.Next
		tail.Next = head
		head = head.Next
		tail.Next.Next = tmp
	}
	return tail
}
```

**方法二：迭代**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func reverseKGroup(head *ListNode, k int) *ListNode {
	dummy := &ListNode{Next: head}
	tmp := dummy

	counts := 1
	for head != nil {
		if counts == k {
			start, end := tmp.Next, head
			head = start
			for start != end {
				next := start.Next
				start.Next = end.Next
				end.Next = start
				start = next
			}
			tmp.Next, tmp = end, head
			counts=0
		}
		head = head.Next
		counts++
	}
	return tmp.Next
}
```