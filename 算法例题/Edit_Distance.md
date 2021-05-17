### 题目描述

**编辑距离**

给你两个单词`word1`和`word2`，请计算出将`word1`转换成`word2`所使用的最少操作数。

操作包括：

- 插入一个字符
- 删除一个字符
- 替换一个字符

**示例**

```bash
输入：word1 = "horse", word2 = "ros"
输出：3
```

### 解法

**解法一**：动态规划

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
func minDistance(word1 string, word2 string) int {
	n, m := len(word1), len(word2)
	dp := make([][]int, n+1)
	for i := 0; i <= n; i++ {
		dp[i] = make([]int, m+1)
		for j := 0; j <= m; j++ {
			if i == 0 {
				dp[i][j] = j
				continue
			}
			if j == 0 {
				dp[i][j] = i
				continue
			}
			if word1[i-1] == word2[j-1] {
				dp[i][j] = dp[i-1][j-1]
			} else {
				dp[i][j] = dp[i-1][j-1] + 1
				if dp[i][j] > dp[i-1][j]+1 {
					dp[i][j] = dp[i-1][j] + 1
				}
				if dp[i][j] > dp[i][j-1]+1 {
					dp[i][j] = dp[i][j-1] + 1
				}
			}
		}
	}
	return dp[n][m]
}
```

