反转链表

----

### 题目描述

> 反转一个链表

示例：
```shell
输入：1 -> 2 -> 3 -> 4 -> 5 -> null
输出：5 -> 4 -> 3 -> 2 -> 1 -> null
```

----

### 解法

方法一：迭代

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func reverseList(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}
	p := reverseList(head.Next)
	head.Next.Next = head
	head.Next = nil
	return p
}
```



方法二：递归

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func reverseList(head *ListNode) *ListNode {
	left := (*ListNode)(nil)
	for head != nil {
		next := head.Next
		head.Next = left
		left = head
		head = next
	}
	return left
}
```