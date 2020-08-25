柱状图中最大的矩形

----

### 题目描述

给定n个非负整数，用来表示柱状图中各个柱子的高度，每个柱子彼此相邻，且宽度为1。

示例：

```shell
输入：[2, 1, 5, 6, 2, 3]
输出：10
```

----

### 解法

解法一：一次循环暴力求解

循环遍历数组每个元素，计算以该元素为左边界的所有面积。

- 时间复杂度：O(n^2)
- 空间复杂度：O(1)

解法二：二次循环暴力求解

循环遍历每个元素，并且查找该元素对应的左右边界，从而求出面积。

- 时间复杂度：O(n^2)
- 空间复杂度：O(1)

```go
func largestRectangleArea(heights []int) int {
	max := 0
	for i, v := range heights {
		left := i
		for left >= 0 && heights[left] >= v {
			left--
		}
		right := i
		for right < len(heights) && heights[right] >= v {
			right++
		}

		area := (right - left - 1) * v
		if area > max {
			max = area
		}
	}
	return max
}
```

解法三：单调栈+一次循环遍历

主要的算法思想为：

使用辅助栈，栈中存储的是数组下标，下标对应的数组元素是递增的。

如果当前元素小于栈顶元素，则当前元素为栈顶元素的右边界，栈中下一个元素为栈顶元素的左边界。

如果当前元素大于等于栈顶元素，则栈顶元素未找到右边界，将当前元素入栈。

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func largestRectangleArea(heights []int) int {
	// 首尾添加0
	nums := make([]int, len(heights)+2)
	for i, v := range heights {
		nums[i+1] = v
	}

	max := 0
	stack := []int{}
	for right := range nums {
        stackLen := len(stack)
		for len(stack) > 0 && nums[stack[stackLen - 1]] > nums[right] {
			h := nums[stack[stackLen - 1]]
			stack = stack[:stackLen-1]

            stackLen = len(stack)
			left := stack[stackLen-1]
			area := (right - left - 1) * h
			if area > max {
				max = area
			}
		}
		stack = append(stack, right)
	}
	return max
}
```

