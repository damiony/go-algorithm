搜索旋转排序数组

----

### 题目描述

给定一个数组`nums`和一个整数`target`。该数组是由升序数组旋转而成，比如`[1,2,3,4,5]`旋转之后可能变成`[4,5,1,2,3]`。

请找出`target`在数组中的索引，如果不存在，返回`-1`。

示例：

```bash
输入：nums = [4, 5, 6, 7, 0, 1, 2], target = 0
输出：4
```

----

### 解法

解法一：二分法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func search(nums []int, target int) int {
	left, right := 0, len(nums)-1
	for left <= right {
		mid := left + (right-left)/2
		if nums[mid] == target {
			return mid
		}
		if nums[mid] < nums[left] {
			if target >= nums[left] || target < nums[mid] {
				right = mid - 1
			} else {
				left = mid + 1
			}
		} else {
            if target >= nums[left] && target < nums[mid] {
                right = mid - 1
            } else {
                left = mid + 1
            }
		}
	}
	return -1
}
```

解法二：二分法2

该解法很巧妙，主要思想是将不包含`target`的另一半数组元素，全部设为无穷大或者无穷小。

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go

func search(nums []int, target int) int {
    left, right := 0, len(nums) - 1
    for left <= right {
        mid := left + (right - left) / 2
        if nums[mid] == target {
            return mid
        }

        if target < nums[0] && nums[mid] >= nums[0] {
            nums[mid] = math.MinInt64
        }

        if target >= nums[0] && nums[mid] < nums[0] {
            nums[mid] = math.MaxInt64
        }

        if nums[mid] < target {
            left = mid + 1
        } else {
            right = mid - 1
        }
    }
    return -1
}
```

