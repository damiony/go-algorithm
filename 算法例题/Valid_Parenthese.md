有效的括号

----

### 题目描述

给定一个只包含`(`，`)`,` {`,` }`,` [`,` ]`的字符串，判断它是否有效。

有效条件：

- 左括号必须用相同类型的右括号闭合
- 左括号必须以正确的顺序闭合

示例：

```shell
输入："()"
输出：true

输入："()[]{}"
输出：true

输入："(["
输出：false

输入："([)]"
输出：false
```

----

### 解法

解法一：利用栈

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func isValid(s string) bool {
	stack := []byte{}
	for i := 0; i < len(s); i++ {
		if s[i] == '(' {
			stack = append(stack, ')')
		} else if s[i] == '{' {
			stack = append(stack, '}')
		} else if s[i] == '[' {
			stack = append(stack, ']')
		} else {
			index := len(stack) - 1
			if index < 0 || stack[index] != s[i] {
				return false
			}
			stack = stack[:index]
		}
	}

	return len(stack) <= 0
}
```

