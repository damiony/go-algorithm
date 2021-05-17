滑动窗口最大值

----

### 题目描述

给定一个数组`nums`，有一个大小为`k`的滑动窗口，从数组的最左侧移动到数组的最右侧，滑动窗口每次只移动一位，返回滑动窗口中的最大值。

示例：

```shell
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
```

----

### 解法

解法一：暴力求解

- 时间复杂度：`O(kN)`
- 空间复杂度：`O(n-k+1)`

```go
func maxSlidingWindow(nums []int, k int) []int {
	var result []int
	for i := 0; i <= len(nums)-k; i++ {
		max := math.MinInt64
		for j := 0; j < k; j++ {
			if nums[i+j] > max {
				max = nums[i+j]
			}
		}
		result = append(result, max)
	}

	return result
}
```

解法二：使用队列求解

构造单调队列，队列中的元素单调递减

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxSlidingWindow(nums []int, k int) []int {
	result := []int{}
	queue := []int{}
	for i := 0; i < len(nums); i++ {
		// 处理窗口中的元素
		for len(queue) > 0 && queue[len(queue)-1] < nums[i] {
			queue = queue[:len(queue)-1]
		}
		queue = append(queue, nums[i])

		if i >= k-1 {
			// 获取当前窗口的最大值
			result = append(result, queue[0])
			// 删除第一个元素
			if len(queue) > 0 && queue[0] == nums[i-k+1] {
				queue = queue[1:]
			}
		}
	}
	return result
}
```

解法三：双数组解法

这个解法很巧妙，但是解释起来也需要花上几句话，人懒直接上代码吧，代码好理解。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxSlidingWindow(nums []int, k int) []int {
	result := []int{}
	if len(nums) <= 0 {
		return result
	}

	left := make([]int, len(nums))
	right := make([]int, len(nums))
	left[0] = nums[0]
	right[len(right)-1] = nums[len(nums)-1]

	for i := 1; i < len(nums); i++ {
		if i%k == 0 {
			left[i] = nums[i]
		} else {
			left[i] = max(left[i-1], nums[i])
		}

		j := len(nums) - i - 1
		if (j+1)%k == 0 {
			right[j] = nums[j]
		} else {
			right[j] = max(right[j+1], nums[j])
		}
	}

	for i := 0; i <= len(nums)-k; i++ {
		result = append(result, max(right[i], left[i+k-1]))
	}
	return result
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

