### 题目描述

**正则表达式匹配**

给你一个字符串`s`和一个字符规律`p`，实现一个支持`.`和`*`的正则表达式匹配。

-  `.`匹配任意单个字符。
- `*`匹配零个或多个前面的那一个元素。

**示例**:

```bash
输入：s = "aa", p = "a"
输出：false
```

### 解法

**解法一**：动态规划

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
func isMatch(s string, p string) bool {
	n, m := len(s), len(p)
	dp := make([][]bool, n+1)
	for i := 0; i <= n; i++ {
		dp[i] = make([]bool, m+1)
		for j := 0; j <= m; j++ {
			if i == 0 && j == 0 {
				dp[i][j] = true
				continue
			}
			if i == 0 {
				if p[j-1] == '*' {
					dp[i][j] = dp[i][j-2]
				}
				continue
			}
			if j == 0 {
				continue
			}
			if p[j-1] == '*' {
				if p[j-2] == '.' || p[j-2] == s[i-1] {
					dp[i][j] = dp[i-1][j] || dp[i][j-2]
				} else {
					dp[i][j] = dp[i][j-2]
				}
			} else if p[j-1] == '.' || s[i-1] == p[j-1] {
				dp[i][j] = dp[i-1][j-1]
			}
		}
	}
	return dp[n][m]
}
```

