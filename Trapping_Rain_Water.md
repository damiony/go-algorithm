接雨水

----

### 题目描述

>  给定用n个非负整数表示的、高度为1的柱子图，计算按此排列的柱子，下雨后可以接多少水

示例：

```shell
输入：[0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
```

----

### 解法

解法一：按列寻找

- 时间复杂度：`O(M * N)`，M是最大高度
- 空间复杂度：`O(1)`

```go
	var maxH int
	for i := 0; i < len(height); i++ {
		if height[i] > maxH {
			maxH = height[i]
		}
	}
	var total int
	for i := 1; i <= maxH; i++ {
		isStart, tmp := false, 0
		for j := 0; j < len(height); j++ {
			if isStart && height[j] < i {
				tmp++
			}
			if height[j] >= i {
				isStart = true
				total += tmp
				tmp = 0
			}
		}
	}
	return total
```

解法二：按行寻找

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(1)`

```go
func trap(height []int) int {
	var total int
	for i := 1; i < len(height)-1; i++ {
		var max_left, max_right int
		for j := i; j >= 0; j-- {
			if height[j] > max_left {
				max_left = height[j]
			}
		}
		for j := i; j < len(height); j++ {
			if height[j] > max_right {
				max_right = height[j]
			}
		}
		if max_left < max_right {
			total += max_left - height[i]
		} else {
			total += max_right - height[i]
		}
	}
	return total
}
```

解法三：动态规划

不解释，看代码

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func trap(height []int) int {
	left := make([]int, len(height))
	right := make([]int, len(height))

	for i := 0; i < len(height); i++ {
		if i == 0 || height[i] > left[i-1] {
			left[i] = height[i]
		} else {
			left[i] = left[i-1]
		}
	}
	for i := len(height) - 1; i >= 0; i-- {
		if i == len(height)-1 || height[i] > right[i+1] {
			right[i] = height[i]
		} else {
			right[i] = right[i+1]
		}
	}

	var total int
	for i := 1; i < len(height)-1; i++ {
		if left[i] > right[i] {
			total += right[i] - height[i]
		} else {
			total += left[i] - height[i]
		}
	}
	return total
}
```

解法四：单调栈

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func trap(height []int) int {
	var total int
	stack := []int{}

	for i := 0; i < len(height); i++ {
		length := len(stack)
		for length > 0 && height[i] > height[stack[length-1]] {
			top := stack[length-1]
			stack = stack[:length-1]
			length--
			if length <= 0 {
				break
			}

			newTop := stack[length-1]
			width := i - newTop - 1
			if height[newTop] > height[i] {
				total += (height[i] - height[top]) * width
			} else {
				total += (height[newTop] - height[top]) * width
			}
		}
		stack = append(stack, i)
	}
	return total
}
```

解法五：双指针法

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
	left, right := 0, len(height)-1
	var maxLeft, maxRight, total int
	for left < right {
		if height[left] < height[right] {
			if height[left] > maxLeft {
				maxLeft = height[left]
			} else {
				total += maxLeft - height[left]
			}
			left++
		} else {
			if height[right] > maxRight {
				maxRight = height[right]
			} else {
				total += maxRight - height[right]
			}
			right--
		}
	}
	return total
```

