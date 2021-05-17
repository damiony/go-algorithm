### 题目描述

**寻找数组的中心索引：**

给定一个整数数组`nums`，返回数组的**中心索引**。

中心索引：数组中心索引左侧的所有元素相加之和，等于右侧所有元素相加之和。

如果中心索引不存在，返回`-1`。

**示例：**

```shell
输入：nums = [1, 7, 3, 6, 5, 6]
输出：3
```

### 解法

**解法一：** 两次循环遍历

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func pivotIndex(nums []int) int {
    sum := 0
    for _, v := range nums {
        sum += v
    }

    left := 0
    for i := range nums {
        if left * 2 + nums[i] == sum {
            return i
        }
        left += nums[i]
    }
    return -1
}
```