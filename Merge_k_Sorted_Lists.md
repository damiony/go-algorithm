### 题目描述

**合并`k`个升序列表：**

给定一个链表数组，每个链表升序排列。

将所有链表合并到一个升序链表中，并且返回。

**示例：**

```shell
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

### 解法

**解法一：顺序遍历**

```go
func mergeKLists(lists []*ListNode) *ListNode {
	// 过滤无效数据
	for j := len(lists) - 1; j >= 0; j-- {
		if lists[j] == nil {
			end := len(lists)
			lists[j], lists[end-1] = lists[end-1], lists[j]
			lists = lists[:end-1]
		}
	}

	dummy := &ListNode{}
	tmp := dummy
	for len(lists) > 0 {
		min := 0
		for i := 0; i < len(lists); i++ {
			if lists[i] != nil && lists[i].Val < lists[min].Val {
				min = i
			}
		}
		tmp.Next = lists[min]
		lists[min] = lists[min].Next
		tmp = tmp.Next

		// 过滤无效数据
		for j := len(lists) - 1; j >= 0; j-- {
			if lists[j] == nil {
				end := len(lists)
				lists[j], lists[end-1] = lists[end-1], lists[j]
				lists = lists[:end-1]
			}
		}
	}
	return dummy.Next
}
```

**解法二：使用最小堆**

略。