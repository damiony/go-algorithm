### 题目描述

**括号生成：**

数字`n`代表生成括号的对数，请设计一个函数，能够生成所有可能的有效括号组合

**示例：**

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

### 解法

**解法一：深度优先遍历（一）**

```go
func generateParenthesis(n int) []string {
	return helper(n, n, "")
}

func helper(left, right int, str string) []string {
	ret := []string{}
	if left == 0 && right == 0 {
		ret = append(ret, str)
	}
	if left < right {
		ret = append(ret, helper(left, right-1, str+")")...)
	}
	if left > 0 {
		ret = append(ret, helper(left-1, right, str+"(")...)
	}
	return ret
}
```

**解法二：深度优先遍历（二）**

```go
func generateParenthesis(n int) []string {
	return generate(n, n)
}

func generate(leftCnt, rightCnt int) []string {
	result := []string{}
	if leftCnt <= 0 && rightCnt <= 0 {
		result = append(result, "")
		return result
	}

	if rightCnt > leftCnt {
		right := generate(leftCnt, rightCnt-1)
		for _, s := range right {
			result = append(result, ")"+s)
		}
	}

	if leftCnt > 0 {
		left := generate(leftCnt-1, rightCnt)
		for _, s := range left {
			result = append(result, "("+s)
		}
	}

	return result
}
```



**解法三：广度优先遍历**

具体思想是使用队列或者栈，`go`代码实现不够简洁，需要记录左右括号的数量，所以省略。

**解法四：动态规划**

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
