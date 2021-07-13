### 题目描述

**跳跃游戏2**

给定一个非负数组，最初位于数组的第一个位置。每个元素表示该位置可以跳跃的最大长度。求跳到最后一个位置所需最少次数。

**示例：**

```shell
输入: [2,3,1,1,4]
输出: 2
```

### 解法

**解法一：贪心算法1**

- 时间复杂度：`O(N)`
- 空间复杂度：`O(1)`

```go
func jump(nums []int) int {
    max := 0
    counts := 0
    remote := 0
    for i := 0; i < len(nums)-1; i++ {
        if max < i+nums[i] {
            max = i+nums[i]
        }
        if i == remote {
            if max == remote {
                return -1
            }
            remote = max
            counts++
        }
    }
    return counts
}
```

解法二：贪心算法2

- 时间复杂度：`O(N^2)`
- 空间复杂度：`O(1)`

```go
func jump(nums []int) int {
    position := len(nums) - 1
    steps := 0
    for position > 0 {
        for i := 0; i < position;i++ {
            if i + nums[i] >= position {
                position = i
                steps++
                break
            }
        }
    }
    return steps
}
```

解法三：暴力循环

- 时间复杂度：`O(N^2)`
- 空间复杂度：`O(N)`

```go
func jump(nums []int) int {
    n := len(nums)
    max := 0
    record := make([]int, n)
    for i := 0; i < n-1; i++ {
        if i > max {
            return -1
        }
        if i+nums[i] > max {
            max = i+nums[i]
        }
        for j := i+1; j <= i+nums[i] && j < n; j++ {
            if record[j] == 0 || record[i]+1 < record[j] {
                record[j] = record[i]+1
            }
        }
    }
    return record[n-1]
}
```

