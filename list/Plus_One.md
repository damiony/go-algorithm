加一

----

### 题目描述

> 给定一个非空数组表示的非负整数，在该数基础上加一

示例：

```shell
输入：[1, 2, 3]
输出：[1, 2, 4]
```

```shell
输入：[9, 9, 9]
输出：[1, 0, 0, 0]
```

----

### 解法

方法一：遍历数组

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func plusOne(digits []int) []int {
    length := len(digits) 
    for i := length - 1; i >= 0; i-- {
        if digits[i] == 9 {
            digits[i] = 0
        } else {
            digits[i]++
            return digits
        }
    }
    digits = make([]int, length + 1)
    digits[0] = 1
    return digits
}
```