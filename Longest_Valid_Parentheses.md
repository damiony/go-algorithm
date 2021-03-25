### 题目描述

**最长有效括号**

给定一个只包含`(`和`)`的字符串，返回最长有效括号子串的长度。

**示例：**

```shell
输入：s = "(()"
输出：2
```

### 解法

**解法一：动态规划**

用`dp[i]`表示以位置`i`结尾的字符拥有的最长子串。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func longestValidParentheses(s string) int {
    max := 0
    n := len(s)
    dp := make([]int, n)
    for i := 1; i < n; i++ {
        ch := s[i]
        if ch == '(' {
            continue
        }
        
        start := i - 1 - dp[i-1]
        if start >= 0 && s[start] == '(' {
            if start > 0 {
                dp[i] = dp[i-1] + dp[start-1] + 2
            } else {
                dp[i] = dp[i-1] + 2
            }
        }

        if max < dp[i] {
            max = dp[i]
        }
    }
    return max
}
```

**解法二：栈**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func longestValidParentheses(s string) int {
	ans := 0
	stack := []int{}
	stack = append(stack, -1)
	for i := range s {
		if s[i] == '(' {
			stack = append(stack, i)
			continue
		}
		stack = stack[:len(stack)-1]
		if len(stack) == 0 {
			stack = append(stack, i)
		} else {
			last := stack[len(stack)-1]
			if ans < i-last {
				ans = i - last
			}
		}
	}
	return ans
}
```

**解法三：两次遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func longestValidParentheses(s string) int {
	left, right := 0, 0
	ans := 0
	for i := 0; i < len(s); i++ {
		if s[i] == ')' {
			right++
		} else {
			left++
		}
		if right == left {
			if ans < left+right {
				ans = left + right
			}
		}
		if right > left {
			right, left = 0, 0
		}
	}
	left, right = 0, 0
	for i := len(s) - 1; i >= 0; i-- {
		if s[i] == ')' {
			right++
		} else {
			left++
		}
		if right == left {
			if ans < left+right {
				ans = left + right
			}
		}
		if right < left {
			right, left = 0, 0
		}
	}
	return ans
}
```