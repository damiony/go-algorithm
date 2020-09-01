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

- 时间复杂度：`O()`
- 空间复杂度：`O()`

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

解法三：