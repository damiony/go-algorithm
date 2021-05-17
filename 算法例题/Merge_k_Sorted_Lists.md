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

- 时间复杂度：`O(kn * k)`, `n`表示链表长度
- 空间复杂度：`O(1)`

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

代码量较多，略。

**解法三：两两合并**

- 时间复杂度：`O(kn * logk)`
- 空间复杂度：`O(logk)`

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func mergeKLists(lists []*ListNode) *ListNode {
    if len(lists) == 0 {
        return nil
    }
    if len(lists) == 1 {
        return lists[0]
    }

    mid := len(lists) >> 1
    left := mergeKLists(lists[0:mid])
    right := mergeKLists(lists[mid:])
    return mergeTwoLists(left, right)
}

func mergeTwoLists(list1 *ListNode, list2 *ListNode) *ListNode {
    dummy := &ListNode{}
    tmp := dummy
    for list1 != nil || list2 != nil {
        if list1 == nil || (list2 != nil && list2.Val < list1.Val) {
            tmp.Next = list2
            list2 = list2.Next
        } else {
            tmp.Next = list1
            list1 = list1.Next
        }
        tmp = tmp.Next
    }
    return dummy.Next
}
```