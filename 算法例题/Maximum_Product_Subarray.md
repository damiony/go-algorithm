乘积最大子数组

----

### 题目描述

给定一个整数数组`nums`，找出数组中乘积最大的连续子数组，并且返回该子数组对应的乘积。

示例：

```bash
输入：[2,3,-2,4]
输出：6
```

----

### 解法

解法一：动态规划1

常规的动态规划解法，复杂度较高 ，可以进一步优化。

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(n)`

```go
func maxProduct(nums []int) int {
	max := math.MinInt64
	dp := make([]int, len(nums))
	for i := 0; i < len(nums); i++ {
		dp[i] = nums[i]
		if dp[i] > max {
			max = dp[i]
		}
	}
	for i := 1; i < len(nums); i++ {
		for j := len(nums) - 1; j >= i; j-- {
			dp[j] = dp[j-1] * nums[j]
			if dp[j] > max {
				max = dp[j]
			}
		}
	}
	return max
}
```

解法二：动态规划2

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go

func maxProduct(nums []int) int {
	minP, maxP, ans := nums[0], nums[0], nums[0]
	for i := 1; i < len(nums); i++ {
		minP, maxP = minNum(minP*nums[i], maxP*nums[i], nums[i]), maxNum(minP*nums[i], maxP*nums[i], nums[i])
		if ans < maxP {
			ans = maxP
		}
	}
	return ans
}

func maxNum(a, b, c int) int {
	max := a
	if max < b {
		max = b
	}
	if max < c {
		max = c
	}
	return max
}

func minNum(a, b, c int) int {
	min := a
	if min > b {
		min = b
	}
	if min > c {
		min = c
	}
	return min
}
```

解法三：使用数学思想

利用负数的个数进行计算。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProduct(nums []int) int {
	pre, ans := 1, nums[0]
	for i := 0; i < len(nums); i++ {
		if nums[i] == 0 {
			pre = 1
			if ans < 0 {
				ans = 0
			}
		} else {
			pre *= nums[i]
			if pre > ans {
				ans = pre
			}
		}
	}

	pre = 1
	for i := len(nums) - 1; i >= 0; i-- {
		if nums[i] == 0 {
			pre = 1
			if ans < 0 {
				ans = 0
			}
		} else {
			pre *= nums[i]
			if pre >= ans {
				ans = pre
			}
		}
	}
	return ans
}
```

