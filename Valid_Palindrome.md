### 题目描述

**验证回文串**

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

假定空字符串是有效的回文串。

**示例**

```bash
输入："A man, a plan, a canal: Panama"
输出：true
```

### 解法

**解法一**：双指针

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func isPalindrome(s string) bool {
    n := len(s)
    if n <= 0 {
        return true
    }
    s = strings.ToLower(s)
    left, right := 0, n - 1
    for left < right {
        if s[left] < '0' || s[left] > '9' && s[left] < 'a' || s[left] > 'z' {
            left++
            continue
        }
        if s[right] < '0' || s[right] > '9' && s[right] < 'a' || s[right] > 'z' {
            right--
            continue
        }
        if s[left] != s[right] {
            return false
        }
        left++
        right--
    }
    return true
}
```

