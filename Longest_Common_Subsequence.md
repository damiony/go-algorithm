最长公共子序列

----

给定两个字符串`text1`和`text2`，返回这两个字符串的最长公共子序列长度。

一个字符串的子序列，是原字符串在不改变字符相对顺序的情况下，删除某些字符或者不删除任何字符，形成的新字符串。

示例：

```bash
输入：text1 = "abcde", text2 = "ace"
输出：3
```

----

### 解法

解法一：动态规划

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go

func longestCommonSubsequence(text1 string, text2 string) int {
	dp := make([][]int, len(text1))
	for i := range dp {
		dp[i] = make([]int, len(text2))
	}

	if text1[0] == text2[0] {
		dp[0][0] = 1
	}
	for i := 1; i < len(text2); i++ {
		if text1[0] == text2[i] {
			dp[0][i] = 1
		} else {
			dp[0][i] = dp[0][i-1]
		}
	}
	for i := 1; i < len(text1); i++ {
		if text1[i] == text2[0] {
			dp[i][0] = 1
		} else {
			dp[i][0] = dp[i-1][0]
		}
	}
	for i := 1; i < len(text1); i++ {
		for j := 1; j < len(text2); j++ {
			if text1[i] == text2[j] {
				// dp[i][j] = max(dp[i][j-1], dp[i-1][j]) + 1
        dp[i][j] = dp[i-1][j-1] + 1
			} else {
				dp[i][j] = max(dp[i][j-1], dp[i-1][j])
			}
		}
	}
	return dp[len(text1)-1][len(text2)-1]
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

