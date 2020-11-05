全排列

----

### 题目描述

给定一个没有重复数字的序列，返回其所有可能的全排列。

示例：

```shell
输入：[1, 2, 3]
输出：
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

----

### 解法

解法一：回溯

- 时间复杂度：`O(n*n!)`
- 空间复杂度：`O(n)`

```go
func permute(nums []int) [][]int {
	res := [][]int{}
	return traceback(nums, res, 0)
}

func traceback(nums []int, res [][]int, start int) [][]int {
	if start == len(nums) {
		tmp := append([]int{}, nums...)
		res = append(res, tmp)
		return res
	}
	for i := start; i < len(nums); i++ {
		nums[start], nums[i] = nums[i], nums[start]
		res = traceback(nums, res, start+1)
		nums[start], nums[i] = nums[i], nums[start]
	}
	return res
}
```

