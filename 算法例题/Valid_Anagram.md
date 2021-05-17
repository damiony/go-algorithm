有效的字母异位词

----

### 题目描述

> 给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的字母异位词。
>
> 字母异位词指字母相同，但排列不同的字符串。

示例1：

```shell
输入：s = "anagram", t = "nagaram"
输出：true
```

示例2：

```shell
输入：s = "rat", t = "car"
输出：false
```



----

### 解法

解法一：排序比较

将两个无序字符串排序，得到两个有序字符串，然后比较字符串是否相等。



解法二：利用数组

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func isAnagram(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}
	tmp := make([]byte, 26, 26)
	for i := 0; i < len(s); i++ {
		tmp[s[i]-'a']++
		tmp[t[i]-'a']--
	}
	for _, v := range tmp {
		if v != 0 {
			return false
		}
	}
	return true
}
```



解法三：利用哈希表

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func isAnagram(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}
	sMap := map[byte]int{}
	for i := 0; i < len(s); i++ {
		sMap[s[i]]++
	}
	for i := 0; i < len(t); i++ {
		if sMap[t[i]] <= 0 {
			return false
		}
		sMap[t[i]]--
	}
	return true
}
```



