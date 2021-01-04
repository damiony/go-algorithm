### 题目描述

**反转字符串2**

给定一个字符串`s`和一个整数`k`，从字符串开头开始，每隔`2k`个字符，对前`k`个字符反转。

- 如果剩余字符少于`k`个，将剩余字符全部反转；
- 如果剩余字符小于`2k`个，但是大于等于`k`个，则反转前`k`个字符，剩余字符保持原样。

**示例**：

```bash
输入：s = "abcdefg", k = 2
输出："bacdfeg"
```

### 解法

**解法一**：暴力反转

- 时间复杂度：`O(n)`
- 空间复杂度：`o(n)`

```go
func reverseStr(s string, k int) string {
	n := len(s)
	if n <= 0 {
		return s
	}

	b := []byte(s)
	start := 0
	for start < n {
		left, right := start, start+k-1
		if right > n-1 {
			right = n - 1
		}
		for left < right {
			b[left], b[right] = b[right], b[left]
			left++
			right--
		}
		start += 2 * k
	}
	return string(b)
}
```

