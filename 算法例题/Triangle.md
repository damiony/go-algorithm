三角形最小路径和

----

### 题目描述

给定一个三角形，自顶向下找出最小路径和。每一步只能移动到下一行中相邻的节点上。

相邻节点是指，本层下标与上一层节点下标相等，或者节点数大1。

示例：

```bash
输入：
[
	[2],
	[3,4],
	[6,5,7],
	[4,1,8,3]
]
输出：11
```

----

### 解法

解法一：动态规划1

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(n*n)`

```go

func minimumTotal(triangle [][]int) int {
	r := len(triangle)
	c := len(triangle[r-1])
	dp := make([][]int, r)
	for i := range dp {
		dp[i] = make([]int, c)
	}

	for i := 0; i < len(triangle[0]); i++ {
		dp[0][i] = triangle[0][i]
	}
	for i := 1; i < r; i++ {
		for j := 0; j < len(triangle[i]); j++ {
			if j == 0 {
				dp[i][j] = triangle[i][j] + dp[i-1][j]
			} else if j >= len(triangle[i-1]) {
				dp[i][j] = triangle[i][j] + dp[i-1][j-1]
			} else {
				dist1 := triangle[i][j] + dp[i-1][j-1]
				dist2 := triangle[i][j] + dp[i-1][j]
				dp[i][j] = dist1
				if dp[i][j] > dist2 {
					dp[i][j] = dist2
				}
			}
		}
	}
	min := dp[r-1][0]
	for i := 1; i < c; i++ {
		if min > dp[r-1][i] {
			min = dp[r-1][i]
		}
	}
	return min
}
```

解法二：动态规划2

- 时间复杂度：`O(n ^ 2)`
- 空间复杂度：`O(n)`

```go
func minimumTotal(triangle [][]int) int {
	r := len(triangle)
	c := len(triangle[r-1])
	dp := make([]int, c)
	for i := 0; i < len(triangle[0]); i++ {
		dp[i] = triangle[0][i]
	}

	for i := 1; i < r; i++ {
		for j := len(triangle[i]) - 1; j >= 0; j-- {
			if j == 0 {
				dp[j] += triangle[i][j]
			} else if j == len(triangle[i])-1 {
				dp[j] += dp[j-1] + triangle[i][j]
			} else {
				dp[j] += triangle[i][j]
				if dp[j] > dp[j-1]+triangle[i][j] {
					dp[j] = dp[j-1] + triangle[i][j]
				}
			}
		}
	}
	min := dp[0]
	for i := 0; i < c; i++ {
		if min > dp[i] {
			min = dp[i]
		}
	}
	return min
}
```

