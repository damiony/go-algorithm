### 题目描述

**回文链表：**

判断一个链表是否为回文链表

**示例：**

```shell
输入：1 -> 2
输出：false
```

### 解法

**解法一：将元素放入数组，再用双指针**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

代码比较简单，省略。

**解法二：递归**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

代码不想写。

**解法三：双指针**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func isPalindrome(head *ListNode) bool {
	if head == nil {
		return true
	}

	// 寻找中位节点
	slow, fast := head, head.Next
	for fast != nil && fast.Next != nil {
		fast = fast.Next.Next
		slow = slow.Next
	}
	// 反转后半部分
	head2 := reverse(slow)
	tail := head2
	// 判断是否回文
	for head != nil && tail != nil {
		if head.Val != tail.Val {
			return false
		}
		tail = tail.Next
		head = head.Next
	}
	// 恢复反转链表
	reverse(head2)
	return true
}

// 翻转链表
func reverse(head *ListNode) *ListNode {
	var dummy *ListNode
	for head != nil {
		next := head.Next
		head.Next = dummy
		dummy = head
		head = next
	}
	return dummy
}
```