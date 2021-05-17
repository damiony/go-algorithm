### 题目描述

**接雨水：**

给定用n个非负整数表示的、高度为1的柱子图，计算按此排列的柱子，下雨后可以接多少水。

**示例：**

```shell
输入：[0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
```

### 解法

**解法一：按行寻找**

- 时间复杂度：`O(M * N)`，M是最大高度
- 空间复杂度：`O(1)`

```go
func trap(height []int) int {
	max := 0
	for _, v := range height {
		if max < v {
			max = v
		}
	}

	total := 0
	// 计算每一行的雨水 再累加
	for h := 1; h <= max; h++ {
		// begin 用来过滤最开始不满足要求的柱子
		begin, tmp := false, 0
		for j := range height {
			// 如果高度足够 累计雨水数
			if height[j] >= h {
				total += tmp
				begin = true
				tmp = 0
			}
			if begin && height[j] < h {
				tmp++
			}
		}
	}

	return total
}
```

**解法二：按列寻找**

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

**解法三：预处理左右边界**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func trap(height []int) int {
	n := len(height)
	// 位置i的左右边界
	left, right := make([]int, n), make([]int, n)
	for i := range height {
		if i == 0 || height[i] > left[i-1] {
			left[i] = height[i]
		} else {
			left[i] = left[i-1]
		}

		if i == 0 || height[n-1-i] > right[n-i] {
			right[n-1-i] = height[n-1-i]
		} else {
			right[n-1-i] = right[n-i]
		}
	}

	total := 0
	for i := 1; i < n-1; i++ {
		if left[i] > right[i] {
			total += right[i] - height[i]
		} else {
			total += left[i] - height[i]
		}
	}

	return total
}
```

**解法四：单调栈**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func trap(height []int) int {
	var total int
	stack := []int{}

	for i := 0; i < len(height); i++ {
    length := len(stack)
    // 位置i作为右边界 计算容器雨水值
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

**解法五：双指针法**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func trap(height []int) int {
	// 使右边界一定大于左边界
	var maxLeft, maxRight, total int
	left, right := 0, len(height)-1
	for left < right {
		if height[left] < height[right] {
			if maxLeft < height[left] {
				maxLeft = height[left]
			} else {
				total += maxLeft - height[left]
			}
			left++
		} else {
			if maxRight < height[right] {
				maxRight = height[right]
			} else {
				total += maxRight - height[right]
			}
			right--
		}
	}
	
	return total
}
```
