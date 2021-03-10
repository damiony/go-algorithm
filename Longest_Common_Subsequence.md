### 题目描述

**最长公共子序列：**

给定两个字符串`text1`和`text2`，返回这两个字符串的最长公共子序列长度。

一个字符串的子序列，是原字符串在不改变字符相对顺序的情况下，删除某些字符或者不删除任何字符，形成的新字符串。

示例：

```shell
输入：text1 = "abcde", text2 = "ace"
输出：3
```

### 解法

**解法一：动态规划**

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
func longestCommonSubsequence(text1 string, text2 string) int {
	n, m := len(text1), len(text2)
	dp := make([][]int, n+1)
	dp[0] = make([]int, m+1)
	for i := 1; i <= n; i++ {
		dp[i] = make([]int, m+1)
		for j := 1; j <= m; j++ {
			if text1[i-1] == text2[j-1] {
				dp[i][j] = dp[i-1][j-1] + 1
				continue
			}
			dp[i][j] = dp[i][j-1]
			if dp[i][j] < dp[i-1][j] {
				dp[i][j] = dp[i-1][j]
			}
		}
	}
	return dp[n][m]
}
```
