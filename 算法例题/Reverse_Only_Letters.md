### 题目描述

**仅仅反转字母**

给定一个字符串`s`，返回反转后的字符串。要求不是字母的字符都保留在原地，而所有字母的位置发生发转。

**示例**

```bash
输入："ab-cd"
输出："dc-ba"
```

### 解法

**解法一：**循环

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func reverseOnlyLetters(S string) string {
	b := []byte(S)
	n := len(b)
	left, right := 0, n-1
	for left < right {
		if b[left] < 'A' || (b[left] > 'Z' && b[left] < 'a') || b[left] > 'z' {
			left++
			continue
		}
		if b[right] < 'A' || (b[right] > 'Z' && b[right] < 'a') || b[right] > 'z' {
			right--
			continue
		}
		b[left], b[right] = b[right], b[left]
		left++
		right--
	}
	return string(b)
}
```

**解法二：**单调栈

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func reverseOnlyLetters(S string) string {
	stack := []byte{}
	b := []byte(S)
	for i := range b {
		if b[i] >= 'A' && b[i] <= 'Z' || b[i] >= 'a' && b[i] <= 'z' {
			stack = append(stack, b[i])
		}
	}
	for i := range b {
		if b[i] >= 'A' && b[i] <= 'Z' || b[i] >= 'a' && b[i] <= 'z' {
			b[i] = stack[len(stack)-1]
			stack = stack[:len(stack)-1]
		}
	}
	return string(b)
}
```

