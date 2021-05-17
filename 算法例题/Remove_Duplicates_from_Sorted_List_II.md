### 题目描述

**删除排序列表中的重复元素2**

给定一个按升序排列的链表，以及链表的头节点`head`，请删除链表中所有存在数字重复情况的节点，只保留原始链表中没有重复出现的数字。

返回结果同样升序排列。

**示例**

```shell
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

### 解法

节点的定义为:

```go
type ListNode struct {
    Val int
    Next *ListNode
}
```

**解法一：双指针**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func deleteDuplicates(head *ListNode) *ListNode {
    dummy := &ListNode{}
    dummy.Next = head

    hair := dummy
    slow, fast := head, head
    for fast != nil {
        for fast.Next != nil && fast.Next.Val == fast.Val {
            fast = fast.Next
        }
        if fast == slow {
            hair.Next = slow
            hair = hair.Next
        }
        fast = fast.Next
        slow = fast
    }

    hair.Next = nil
    return dummy.Next
}
```