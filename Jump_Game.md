跳跃游戏

----

### 题目描述

给定一个非负整数，最初位于整数的第一个位置。数组中的每个元素代表你在该位置可以跳跃的最大长度。

请判断能否到达最后一个位置。

示例：

```bash
输入：[2, 3, 1, 1, 4]
输出：true
```

----

### 解法

解法一：`DFS`

从最后一个位置开始倒推。

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func canJump(nums []int) bool {
	visited := make([]bool, len(nums))
	return helper(nums, 1, visited)
}

func helper(nums []int, end int, visited []bool) bool {
	if end == len(nums) {
		return true
	}

	distance := 0
	for i := len(nums) - 1 - end; i >= 0; i-- {
		distance++
		if visited[i] {
			continue
		}
		if distance > nums[i] {
			continue
		}
		if helper(nums, distance+end, visited) {
			return true
		}
		visited[i] = true
	}
	return false
}
```

解法二：贪心

- 时间复杂度：`O(N)`
- 空间复杂度：`O(1)`

```go
func canJump(nums []int) bool {
	maxDistance := 0
	for i := 0; i < len(nums) - 1; i++ {
		if i > maxDistance {
			break
		}
		if i+nums[i] > maxDistance {
			maxDistance = nums[i] + i
		}
	}
	if maxDistance >= len(nums) - 1 {
		return true
	}
	return false
}
```

