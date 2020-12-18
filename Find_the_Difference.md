## 题目描述

**找不同**

给定两个字符串`s`和`t`，它们只包含小写字母。

字符串`t`由字符串`s`随机重排，然后在随机位置添加一个字母。找出被添加的字母。

示例：

```bash
输入：s = "abdc", t = "abcde"
输出："e"
```

## 解法

**解法一：计数法**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func findTheDifference(s string, t string) byte {
    m := map[byte]int{}
    for i := 0; i < len(s); i++ {
        m[s[i]]++
    }
    for i := 0; i < len(t); i++ {
        if m[t[i]] <= 0 {
            return t[i]
        } else {
            m[t[i]]--
        }
    }
    return 0
}
```

**解法二：求和**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func findTheDifference(s string, t string) byte {
    var ans byte = 0
    for i := 0; i < len(t); i++ {
        ans += t[i]
    }
    for i := 0; i < len(s); i++ {
        ans -= s[i]
    }
    return ans
}
```

**解法三：位运算**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func findTheDifference(s string, t string) byte {
    var ans byte
    for i := range s {
        ans ^= s[i] ^ t[i]
    }
    ans ^= t[len(t)-1]
    return ans
}
```

