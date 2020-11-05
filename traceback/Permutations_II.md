全排列 II

----

### 题目描述

给定一个可包含重复数字的序列`nums`，按任意顺序返回所有不重复的全排列。

示例：

```shell
输入：nums = [1, 1, 2]
输出：
[
	[1,1,2],
	[1,2,1],
	[2,1,1]
]
```

----

### 解法

解法一：回溯法（1）

```go
func permuteUnique(nums []int) [][]int {
	sort.Ints(nums)
	res := [][]int{}
	history := make([]bool, len(nums))
	maybe := []int{}
	return tracebackUnique(nums, maybe, res, history)
}

func tracebackUnique(nums []int, maybe []int, res [][]int, history []bool) [][]int {
	if len(maybe) == len(nums) {
		res = append(res, append([]int{}, maybe...))
		return res
	}

	for i, v := range nums {
		if history[i] || i > 0 && !history[i-1] && v == nums[i-1] {
			continue
		}

		history[i] = true
		maybe = append(maybe, v)
		res = tracebackUnique(nums, maybe, res, history)

		history[i] = false
		maybe = maybe[:len(maybe)-1]
	}
	return res
}
```



解法二：回溯法（2）

```go
func permuteUnique(nums []int) [][]int {
	sort.Ints(nums)
	res := [][]int{}
	return tracebackUnique(nums, res, 0)
}

func tracebackUnique(nums []int, res [][]int, start int) [][]int {
	if start == len(nums) {
		res = append(res, nums)
		return res
	}

	for i := start; i < len(nums); i++ {
		if i != start && nums[i] == nums[start] {
			continue
		}
		nums[start], nums[i] = nums[i], nums[start]

		tmp := append([]int{}, nums...)
		res = tracebackUnique(tmp, res, start+1)
	}
	return res
}
```

