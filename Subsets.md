### 题目描述

**子集：**

给定一组不含重复元素的整数数组`nums`，返回该数组所有可能的子集（幂集）

**示例：**

```shell
输入：nums = [1,2,3]
输出：
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

### 解法

**解法一：递归**

- 时间复杂度：`O(N*2^N)`
- 空间复杂度：`O(N)`

```go
func subsets(nums []int) [][]int {
	result := [][]int{}
	sub := []int{}
	result = append(result, sub)
	return recursion(nums, 0, result)
}

func recursion(nums []int, i int, result [][]int) [][]int {
	if i >= len(nums) {
		return result
	}
	size := len(result)
	for j := 0; j < size; j++ {
		sub := []int{}
		sub = append(sub, result[j]...)
		sub = append(sub, nums[i])
		result = append(result, sub)
	}
	return recursion(nums, i+1, result)
}
```

**解法二：迭代**

- 时间复杂度：`O(N*2^N)`
- 空间复杂度：`O(1)`

```go
func subsets(nums []int) [][]int {
	result := [][]int{}
	sub := []int{}

	result = append(result, sub)
	for i := 0; i < len(nums); i++ {
		size := len(result)
		for j := 0; j < size; j++ {
			sub = []int{}
			sub = append(sub, result[j]...)
			sub = append(sub, nums[i])
			result = append(result, sub)
		}
	}
	return result
}
```

**解法三：回溯法**

- 时间复杂度：`O(2^N)`
- 空间复杂度：`O(N)(此处是通常的空间复杂度，还得依赖具体语言的实现)`

```go
func subsets(nums []int) [][]int {
	result := [][]int{}
	sub := []int{}
	return backtrack(nums, 0, result, sub)
}

func backtrack(nums []int, j int, result [][]int, sub []int) [][]int {
	sub = append([]int{}, sub...)
	if j == len(nums) {
		result = append(result, sub)
		return result
	}
	sub = append(sub, nums[j])
	result = backtrack(nums, j+1, result, sub)

	sub = sub[:len(sub)-1]
	result = backtrack(nums, j+1, result, sub)

	return result
}
```

**解法四：二进制位法**

- 时间复杂度：`O(N*2^N)`
- 空间复杂度：`O(1)`

```go

func subsets(nums []int) [][]int {
	result := [][]int{}
	for i := 0; i < (1 << len(nums)); i++ {
		layer := []int{}
		for j := 0; j < len(nums); j++ {
			if (1 << j) > i {
				break
			}
			if (i>>j)&1 == 1 {
				layer = append(layer, nums[j])
			}
		}
		result = append(result, layer)
	}
	return result
}
```