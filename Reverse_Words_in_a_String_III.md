### 题目描述

**反转字符串中的单词3**

给定一个字符串，请反转字符串中每个单词的字符顺序，同时保留空格和单词的初始顺序。

**示例**

```bash
输入："Let's take Leetcode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```

### 解法

**解法一**：一次遍历 + 双指针

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func reverseWords(s string) string {
	n := len(s)
	b := []byte(s)
	wl, wr := 0, 0
	for wr < n {
		if wr > 0 && b[wr-1] == ' ' {
			wl = wr
		}
		if wr == n-1 || b[wr+1] == ' ' {
			left, right := wl, wr
			for left < right {
				b[left], b[right] = b[right], b[left]
				left++
				right--
			}
		}
		wr++
	}
	return string(b)
}
```

