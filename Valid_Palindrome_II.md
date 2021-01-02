### 题目描述

**验证回文字符串2**

给定一个非空字符串`s`，最多删除一个字符，判断是否能成为回文字符串。

**示例**

```bash
输入："abca"
输出：true
```

### 解法

**解法一**：双指针

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func validPalindrome(s string) bool {
    left, right := 0, len(s) - 1
    for left < right {
        if s[left] == s[right] {
            left++
            right--
            continue
        }

        
        flag := true
        for i, j := left+1, right; i < j; i, j = i+1, j-1 {
            if s[i] != s[j] {
                flag = false
                break
            }
        }
        if flag {
            return flag
        }
        flag = true
        for i, j := left, right-1; i < j; i, j = i+1, j-1 {
            if s[i] != s[j] {
                flag = false
            }
        }
        return flag
    }
    return true
}
```





