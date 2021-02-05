### 题目描述

**环形链表 II：**

给定一个链表，返回链表开始入环的第一个节点，如果链表无环，返回null

**示例：**

```shell
输入：head = [3, 2, 0, -4], pos = 1, pos表示入环点
输出：tail connects to node index 1
```

```shell
输入：head = [1, 2], pos = 0
输出：tail connects to node index 0
```

```shell
输入：head = [1], pos = -1
输出：no cycle
```

### 解法

**方法一：哈希表法**

遍历节点，同时，在哈希表检查是否存在相同指针，如果存在则返回。
如果不存在，将当前节点存入哈希表。
直到节点遍历完，或者找到入环点。

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func detectCycle(head *ListNode) *ListNode {
	nodeM := make(map[*ListNode]int)

	for head != nil {
		if _, ok := nodeM[head]; ok {
			return head
		}
		nodeM[head] = 1

		head = head.Next
	}

	return nil
}
```

**方法二：快慢指针**

快指针走两步，慢指针走一步，如果未找到相遇点，则不成环。
如果找到相遇点，此时相遇点一定在环内，保存相遇点。
使用两个指针，分别指向相遇点和链表头，每次都只走一步，直到相遇，即可找到入环点。

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func detectCycle(head *ListNode) *ListNode {
	slow, fast := head, head
	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
		// 如果找到相遇点
		if slow == fast {
			// 一个指针指向链表头
			// 一个指针指向相遇点
			// 每次走一步 直到相遇
			fast = head
			for slow != fast {
				slow = slow.Next
				fast = fast.Next
			}
			return slow
		}
	}
	return nil
}
```