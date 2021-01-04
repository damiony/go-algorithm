### 题目描述

**字符串中的第一个唯一字符**

给定一个字符串，找到它的第一个不重复字符，并返回它的索引。如果不存在，返回-1。

**示例**:

```bash
输入：s = "leetcode"
输出：0
```

### 解法

**解法一**：数组

- 时间复杂度：`O(n)`
- 空间复杂度：`O(m)`，m表示字符集的大小

```go
func firstUniqChar(s string) int {
	flag := [26]int{}
	for i := range s {
		ch := s[i] - 'a'
		flag[ch]++
	}
	for i := range s {
		ch := s[i] - 'a'
		if flag[ch] == 1 {
			return i
		}
	}
	return -1
}
```

