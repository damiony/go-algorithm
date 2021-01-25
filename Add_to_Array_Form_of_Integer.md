### 题目描述

**数组形式的整数加法**

对于非负整数`X`而言，`X`的数组形式是每位数字按从左到右顺序形成的数组。例如`1231`的数组形式为`[1,2,3,4]`。

给定非负整数`X`的数组形式`A`，返回整数`X+K`的数组形式。

**示例：**

```shell
输入：A = [1, 2, 0, 0], K = 34
输出：[1, 2, 3, 4]
```

### 解法

**解法一：**

- 时间复杂度：`O(max(m, n))`，`m`表示`A`的长度，`n`表示`K`的长度
- 空间复杂度：`O(max(m, n))`

```go
func addToArrayForm(A []int, K int) []int {
    for i := len(A) - 1; i >= 0; i-- {
        A[i] += K % 10
        K /= 10
        if A[i] >= 10 {
            K++
            A[i] -= 10
        }
    }

    for K > 0 {
        A = append([]int{K % 10}, A...)
        K /= 10
    }

    return A
}
```

**解法二：**

- 时间复杂度：`O(max(m, n))`
- 空间复杂度：`O(max(m, n))`

```go
func addToArrayForm(A []int, K int) []int {
    for i := len(A) - 1; i >= 0; i-- {
        K += A[i]
        A[i] = K % 10
        K /= 10
    }

    for K > 0 {
        A = append([]int{K % 10}, A...)
        K /= 10
    }
    return A
}
```