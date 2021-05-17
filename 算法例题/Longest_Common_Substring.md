### 题目描述

**最长公共子串：**

给定两个字符串`str1`和`str2`，输出最长公共子串。如果不存在，输出`-1`。

**示例：**

```shell
输入：str1 = "1AB2345CD", str2 = "12345EF"
输出："2345"
```

### 解法

**解法一：动态规划**

- 时间复杂度：`O(m * n)`
- 空间复杂度：`O(min(m, n))`

```go
func LCS(str1 string, str2 string) string {
  // dp[i][j] 表示以 i-1 和 j-1 结尾的最长公共子串
  // 双数组可以优化成单数组
	n1, n2 := len(str1), len(str2)
	if n1 < n2 {
		str1, str2 = str2, str1
		n1, n2 = n2, n1
	}

	dp := make([]int, n2+1)
	max, end := 0, 0
	for i := 1; i <= n1; i++ {
		for j := n2; j >= 1; j-- {
			if str1[i-1] == str2[j-1] {
				dp[j] = dp[j-1] + 1
			} else {
				dp[j] = 0
			}
			if max < dp[j] {
				max = dp[j]
				end = i
			}
		}
	}
	if max == 0 {
		return "-1"
	}
	return str1[end-max : end]
}
```