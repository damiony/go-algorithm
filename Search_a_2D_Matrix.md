搜索二维矩阵

----

### 题目描述

编写一个高效的算法，来判断`mxn`矩阵中，是否存在一个目标值。该矩阵具有如下特性：

- 每行中的整数从左到右按升序排列。
- 每行的第一个整数大于前一行的最后一个整数。

示例：

```bash
输入：matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,50]], target = 3
输出：true
```

----

### 解法：

解法一：二分法

二分法的主要思想就是：将该二维矩阵视为一维有序数组。

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func searchMatrix(matrix [][]int, target int) bool {
	if len(matrix) == 0 || len(matrix[0]) == 0 {
		return false
	}
	r, c := len(matrix), len(matrix[0])
	left, right := 0, r*c-1
	for left <= right {
		mid := left + (right-left)/2
		midV := matrix[mid/c][mid%c]
		if midV == target {
			return true
		}
		if midV > target {
			right = mid - 1
		} else {
			left = mid + 1
		}
	}
	return false
}
```

