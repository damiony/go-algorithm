### 题目描述

数字`n`代表生成括号的对数，请设计一个函数，能够生成所有可能的有效括号组合

示例：

```shell
输入：n = 3
输出：
[
	"((()))",
	"(()())",
	"(())()",
	"()(())",
	"()()()"
]
```

----

### 解法

解法一：深度优先遍历

```go
func generateParenthesis(n int) []string {
	result := []string{}
	return generate("", n, n, result)
}

func generate(current string, leftCnt, rightCnt int, result []string) []string {
	if leftCnt <= 0 && rightCnt <= 0 {
		result = append(result, current)
		return result
	}

	if rightCnt > leftCnt {
		result = generate(current+")", leftCnt, rightCnt-1, result)
	}

	if leftCnt > 0 {
		result = generate(current+"(", leftCnt-1, rightCnt, result)
	}

	return result
}
```



解法二：广度优先遍历

代码写不动了，等哪天养好精神再来。



解法三：动态规划

- 思想

  第`i`对括号组合可以由第`i-1`对括号组成。

  动态规划方程为：`第i对括号组合 = "(" + 可能的括号组合 + ")" + 剩下的括号组合`。

```go
func generateParenthesis(n int) []string {
	dp := [][]string{}
	if n <= 0 {
		return []string{}
	}

	dp = append(dp, []string{""})
	for i := 1; i <= n; i++ {
		result := []string{}
		for j := 0; j < i; j++ {
			leftDp := dp[j]
			rightDp := dp[i-1-j]
			for _, left := range leftDp {
				for _, right := range rightDp {
					result = append(result, "("+left+")"+right)
				}
			}
		}
		dp = append(dp, result)
	}
	return dp[n]
}
```

