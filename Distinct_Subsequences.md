### 题目描述

**不同的子序列**

给定一个字符串`s`和一个字符串`t`，计算在`s`的子序列中`t`出现的个数。

**示例**：

```bash
输入：s = "rabbbit", t = "rabbit"
输出：3
```

### 解法

**解法一**：动态规划

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
func numDistinct(s string, t string) int {
	n, m := len(t), len(s)
	dp := make([][]int, n+1)
	for i := 0; i <= n; i++ {
		dp[i] = make([]int, m+1)
		for j := i; j <= m; j++ {
			if i == 0 {
				dp[i][j] = 1
				continue
			}
			if s[j-1] == t[i-1] {
				dp[i][j] = dp[i-1][j-1] + dp[i][j-1]
			} else {
				dp[i][j] = dp[i][j-1]
			}
		}
	}
	return dp[n][m]
}
```

