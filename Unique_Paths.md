不同路径

----

### 题目描述

一个机器人位于`mxn`网格的左上角，`m`代表列，`n`代表行。每次只能向下或者向右移动一步，达到网格右下角总共有多少条不同的路径？

示例：

```bash
输入：m = 3, n = 7
输出：28
```

----

### 解法

解法一：动态规划1

- 时间复杂度：`O(m * n)`
- 空间复杂度：`O(m * n)`

```go
func uniquePaths(m int, n int) int {
	dp := make([][]int, n)
	for i := range dp {
		dp[i] = make([]int, m)
	}
	dp[0][0] = 1
	for i := 1; i < n; i++ {
		dp[i][0] = 1
	}
	for i := 1; i < m; i++ {
		dp[0][i] = 1
	}
	for i := 1; i < n; i++ {
		for j := 1; j < m; j++ {
			dp[i][j] = dp[i-1][j] + dp[i][j-1]
		}
	}
	return dp[n-1][m-1]
}
```

解法二：动态规划2

- 时间复杂度：`O(m * n)`
- 空间复杂度：`O(1)`

```go
func uniquePaths(m int, n int) int {
    if m > n {
        m, n = n, m
    }
	dp := make([]int, m)
	for i := 0; i < m; i++ {
		dp[i] = 1
	}
	for i := 1; i < n; i++ {
		for j := 1; j < m; j++ {
			dp[j] += dp[j-1]
		}
	}
	return dp[m-1]
}
```

解法三：公式法

- 时间复杂度：`O(min(m, n))`
- 空间复杂度：`O(1)`

```go
func uniquePaths(m int, n int) int {
    return int(new(big.Int).Binomial(int64(m+n-2), int64(n-1)).Int64())
}
```



