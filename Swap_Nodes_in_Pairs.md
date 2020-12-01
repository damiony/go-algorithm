两两交换链表的节点

----

### 题目描述

> 给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。
>
> 你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

示例：

```shell
输入：1 -> 2 -> 3 -> 4
输出：2 -> 1 -> 4 -> 3
```

----

### 解法

方法一：递归

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func swapPairs(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }
    first := head
    second := head.Next

    first.Next = swapPairs(second.Next)
    second.Next = first
    return second
}
```



方法二：迭代

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func swapPairs(head *ListNode) *ListNode {
	dummy := &ListNode{Next: head}
	tmp := dummy

	for tmp.Next != nil && tmp.Next.Next != nil {
		first := tmp.Next
		second := first.Next

		first.Next = second.Next
		second.Next = first
		tmp.Next = second

		tmp = first
	}

	return dummy.Next
}
```