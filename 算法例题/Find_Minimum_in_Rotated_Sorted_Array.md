寻找旋转排序数组中的最小值

----

### 题目描述

将升序排列的有序数组，在某个点上进行旋转。找出旋转后数组中的最小元素。

示例：

```bash
输入：nums = [3, 4, 5, 1, 2]
输出：1
```

----

### 解法

解法一：二分法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func findMin(nums []int) int {
	left, right := 0, len(nums)-1
	for left < right {
		mid := left + (right-left)/2
		if nums[mid] < nums[right] {
            right = mid
		} else {
			left = mid + 1
		}
	}
	return nums[left]
}
```

