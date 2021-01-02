### 题目描述

**字母异位词分组**

给定一个字符串数组，将字母异位词组合在一起，字母异位词是指字母相同，但排列不同的字符串。

假定字符串只包含小写字母。

**示例**

```bash
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

### 解法

**解法一**：排序

- 时间复杂度：`O(nklogk)`
- 空间复杂度：`O(nk)`

```go
func groupAnagrams(strs []string) [][]string {
	m := map[string][]string{}
	for _, str := range strs {
		b := []byte(str)
		sort.Slice(b, func(i, j int) bool { return b[i] < b[j] })
		sortedStr := string(b)
		m[sortedStr] = append(m[sortedStr], str)
	}
	ans := [][]string{}
	for _, v := range m {
		ans = append(ans, v)
	}
	return ans
}
```

**解法二**：数组

- 时间复杂度：`O(n(k + m))`，k表示最长字符长度，m表示字符集长度
- 空间复杂度:`O(n(k + m))`

```go
func groupAnagrams(strs []string) [][]string {
	m := map[[26]int][]string{}
	for _, str := range strs {
		arr := [26]int{}
		for i := 0; i < len(str); i++ {
			arr[str[i]-'a']++
		}
		m[arr] = append(m[arr], str)
	}
	ans := [][]string{}
	for _, v := range m {
		ans = append(ans, v)
	}
	return ans
}
```

**解法三**：质数

质数是指：一个大于1的自然数，除了1和它本身之外，不能被其他自然数整除。

性质：任何一个大于1的自然数都可以分解成几个质数连乘积的形式，且这种分解是唯一的。

- 时间复杂度：`O(nk)`
- 空间复杂度：`O(nk)`

```go
func groupAnagrams(strs []string) [][]string {
	m := map[int][]string{}
	prime := []int{2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97, 101}
	for _, str := range strs {
		tmp := 1
		for i := 0; i < len(str); i++ {
			tmp *= prime[str[i]-'a']
		}
		m[tmp] = append(m[tmp], str)
	}
	ans := [][]string{}
	for _, v := range m {
		ans = append(ans, v)
	}
	return ans
}
```

