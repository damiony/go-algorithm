### 题目描述

**找到字符串中所有字母异位词**

给定一个字符串`s`和一个非空字符串`p`，找到`s`中所有是`p`的字母异位词的子串，返回这些子串的起始索引。

假定字符串只包含小写英文字母。

**示例**

```bash
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

### 解法

**解法一**：暴力循环

- 时间复杂度：`o(slength * plength)`
- 空间复杂度：`O(m)`，m表示字符集的大小。

```go
func findAnagrams(s string, p string) []int {
	ans := []int{}
	for i := 0; i < len(s)-len(p)+1; i++ {
		tmp := make([]int, 26)
		for j := 0; j < len(p); j++ {
			tmp[p[j]-'a']++
		}
		ok := true
		for j := i; j < i+len(p); j++ {
			ch := s[j] - 'a'
			tmp[ch]--
			if tmp[ch] < 0 {
				ok = false
			}
		}
		if ok {
			ans = append(ans, i)
		}
	}
	return ans
}
```

**解法二**：滑动窗口

- 时间复杂度：`O(n)`
- 空间复杂度：`O(m)`，m表示字符集的大小

```go
func findAnagrams(s string, p string) []int {
	ans := []int{}
	left, right := 0, 0
	pcnt := make([]int, 26)
	for i := 0; i < len(p); i++ {
		pcnt[p[i]-'a']++
	}
	scnt := make([]int, 26)
	for right < len(s) {
		ch := s[right] - 'a'
		scnt[ch]++
		if scnt[ch] > pcnt[ch] {
			for i := left; i <= right; i++ {
				scnt[s[i]-'a']--
				if s[i] == s[right] {
					left = i + 1
					break
				}
			}
		}
		right++

		if right-left == len(p) {
			ans = append(ans, left)
		}
	}
	return ans
}
```

