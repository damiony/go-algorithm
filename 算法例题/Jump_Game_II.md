跳跃游戏2

----

### 题目描述

给定一个非负数组，最初位于数组的第一个位置。每个元素表示该位置可以跳跃的最大长度。求跳到最后一个位置所需最少次数。

示例：

```bash
输入: [2,3,1,1,4]
输出: 2
```

----

### 解法

解法一：贪心算法1

- 时间复杂度：`O(N)`
- 空间复杂度：`O(1)`

```go
func jump(nums []int) int {
    end := 0
    max := 0
    steps := 0
    for i := 0; i < len(nums) - 1; i++ {
        if max < i + nums[i] {
            max = i + nums[i]
        }
        if i == end {
            end = max
            steps++
        }
    }
    return steps
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
	record := make([]int, len(nums))
	maxDistance, index := 0, 0
	for index < len(nums)-1 {
		if index+nums[index] > maxDistance {
			maxDistance = index + nums[index]
		}
		for j := index + 1; j <= index+nums[index] && j < len(nums); j++ {
			if record[j] == 0 || record[index]+1 < record[j] {
				record[j] = record[index] + 1
			}
		}
		index++
	}
	return record[len(record)-1]
}
```

