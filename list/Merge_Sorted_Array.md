合并两个有序数组

----

### 题目描述

> 给定两个数组`nums1`和`nums2`，将`nums2`合并到`nums1`，使`nums1`成为一个有序数组。

示例:

```shell
输入：
nums1 = [1, 2, 3, 0, 0, 0], m = 3
nums2 = [2, 5, 6], n = 3

输出：
[1,2,2,3,5,6]
```

----

### 解法

方法一：合并 + 排序

合并数组并排序

- 时间复杂度：O(log(n + m))
- 空间复杂度：O(1)

代码略

方法二：从头遍历数组

将`nums1`复制到新数组中，然后遍历`nums2`和新数组，将值组合到`nums1`中。

- 时间复杂度：O(m + n)
- 空间复杂度：O(m)

代码略

方法三：从尾遍历数组

- 时间复杂度：O(m+n)
- 空间复杂度：O(1)

```go
func merge(nums1 []int, m int, nums2 []int, n int)  {
	l1 := m - 1
	l2 := n - 1
	l3 := m + n - 1
	for l1 >= 0 && l2 >= 0 {
		if nums1[l1] > nums2[l2] {
			nums1[l3] = nums1[l1]
			l1--
		} else {
			nums1[l3] = nums2[l2]
			l2--
		}
		l3--
	}

	if l1 < 0 {
		copy(nums1, nums2[:l2+1])
	}
	return
}
```