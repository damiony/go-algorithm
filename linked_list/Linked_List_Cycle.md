环形链表

----

### 题目描述

> 给定一个链表，判断链表中是否有环

示例：

```shell
输入：head = [3, 2, 0, -4], pos = 1, pos表示链接到链表中的位置
输出：true
解释：链表中有一个环
```

```shell
输入：head = [1], pos = -1
输出：false
解释：链表中没有环
```

----

### 解法

方法一：**利用哈希表**

每遍历一个节点，就从哈希表中检查是否存在该指针，如果存在则表示有环。如果不存在，则将该节点添加到哈希表。直到遍历结束。

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func hasCycle(head *ListNode) bool {
	nodeM := make(map[*ListNode]int)

	for head != nil {
		if _, ok := nodeM[head]; ok {
			return true
		}
		nodeM[head] = 1
		head = head.Next
	}
	return false
}
```



方法二：**快慢指针**

- 时间复杂度：
- 空间复杂度：

```go
func hasCycle(head *ListNode) bool {
    stepOne := head
    stepTwo := head

    for stepTwo != nil && stepTwo.Next != nil {
        stepOne = stepOne.Next
        stepTwo = stepTwo.Next.Next

        if stepOne == stepTwo {
            return true
        }
    }

    return false
}
```