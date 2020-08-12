删除排序数组中的重复项

----

### 题目描述

> 给定一个数组，原地删除重复出现的元素，返回新数组的长度
>
> 要求空间复杂度为 O(1)

示例：

```shell
输入：[1, 1, 2]
输出：2
```

----

### 解法

方法一：快慢指针

解法很容易，看代码就能理解

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func removeDuplicates(nums []int) int {
	if len(nums) == 0 {
		return 0
	}
	left := 0
	right := 0

	for right < len(nums) {
		if nums[right] == nums[left] {
			right++
		} else {
			left++
			nums[left] = nums[right]
			right++
		}
	}
	return left + 1
}
```

方法二：记录重复数据个数

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func removeDuplicates(nums []int) int {
	counts := 0
	for i := 1; i < len(nums); i++ {
		if nums[i] == nums[i-1] {
			counts++
		} else {
			nums[i-counts] = nums[i]
		}
	}
	return len(nums) - counts
}
```