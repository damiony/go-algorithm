### 题目描述

**搜索插入位置：**

给定一个排序数组和一个目标值，在数组中找到目标值，并且返回下标。如果目标值不存在，返回将它按序插入时对应的位置。

**示例：**

```shell
输入：[1, 3, 5, 6], 5
输出：2
```

### 解法

**思想：**

查找第一个大于等于`target`的值的下标

**解法一：** 二分法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func searchInsert(nums []int, target int) int {
    left, right := 0, len(nums) - 1
    for left <= right {
        mid := left + (right - left) >> 1
        if nums[mid] == target {
            return mid
        }
        if nums[mid] > target {
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
    return right + 1
}
```