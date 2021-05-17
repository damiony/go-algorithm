### 题目描述

**反转链表2：**

给定头指针`head`和两个整数`left`和`right`，其中`left <= right`。反转从`left`到`right`的位置，并且返回。

**示例：**

```shell
输入：head = [1,2,3,4,5], left=2, right=4
输出：[1,4,3,2,5]
```

```shell
输入：head = [5], left = 1, right = 1
输出：[5]
```

### 解法

**解法一：**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func reverseBetween(head *ListNode, left int, right int) *ListNode {
    dummy := &ListNode{
        Next: head,
    }
    // 搜索反转范围
    pre, tail := dummy, head
    counts := 1
    for tail != nil && counts < right {
        if counts < left {
            pre = pre.Next
        }
        tail = tail.Next
        counts++
    }
    // 在指定范围内反转
    head = pre.Next
    var next *ListNode
    for head != tail {
        next = head.Next
        head.Next = tail.Next
        tail.Next = head
        head = next
    }

    pre.Next = tail
    return dummy.Next
}
```