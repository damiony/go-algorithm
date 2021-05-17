### 题目描述

**同构字符串**

给定两个字符串`s`和`t`，判断它们是否是同构的。如果`s`中的字符可以按某种映射关系替换得到`t`，那么这两个字符串是同构的。

每个出现的字符都应当映射到另一个字符，同时不改变字符的顺序。不同字符不能映射到同一个字符上，相同字符只能映射到同一个字符上，字符可以映射到自己本身。

**示例**

```bash
输入：s = "egg", t = "add"
输出：true
```

### 解法

**解法一**：

```go
func isIsomorphic(s string, t string) bool {
	if len(s) != len(t) {
		return false
	}
	l1, l2 := make([]int, 128), make([]int, 128)
	for i := range s {
		if l1[s[i]] != l2[t[i]] {
			return false
		}
		l1[s[i]] = i+1
		l2[t[i]] = i+1
	}
	return true
}
```

**解法二：**

```bash
func isIsomorphic(s string, t string) bool {
	if len(s) != len(t) {
		return false

	}
	l1, l2 := make([]int, 128), make([]bool, 128)
	for i := range s {
		if l1[s[i]] == 0 {
			if l2[t[i]] {
				return false
			}
			l1[s[i]] = int(t[i]) + 1
			l2[t[i]] = true
		} else {
			if l1[s[i]] != int(t[i])+1 {
				return false
			}
		}
	}
	return true
}
```

**解法三：**

```bash
func isIsomorphic(s string, t string) bool {
    if len(s) != len(t) {
        return false
    }
    m1 := map[byte]byte{}
    m2 := map[byte]byte{}
    for i := range s {
        a, ok := m1[s[i]]
        if ok && a != t[i] {
            return false
        }
        if !ok {
            _, ok = m2[t[i]]
            if ok {
                return false
            }
            m1[s[i]] = t[i]
            m2[t[i]] = s[i]
        }
    }
    return true
}
```

