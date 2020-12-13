打家劫舍

----

### 题目描述

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

示例：

```bash
输入：[1,2,3,1]
输出：4
```

----

### 解法

解法一：回溯法

- 时间复杂度：`O(2^n)`
- 空间复杂度：`O(1ogn)`

```go
func rob(nums []int) int {
	status, index, money, max := 0, 0, 0, 0
	return dfs(nums, status, index, money, max)
}

func dfs(nums []int, status, index, money, max int) int {
	if index == len(nums) {
		if max < money {
			max = money
		}
		return max
	}

	m1, m2 := 0, 0
	m1 = dfs(nums, 0, index+1, money, max)
	if status == 0 {
		m2 = dfs(nums, 1, index+1, money+nums[index], max)
	}

	if m1 > m2 {
		return m1
	}
	return m2
}
```

解法二：动态规划

- 时间复杂度：`o(n)`
- 空间复杂度：`O(1)`

```go
func rob(nums []int) int {
	cur := []int{0, 0} // 不偷 偷
	for _, v := range nums {
		m1 := cur[1]
		if m1 < cur[0] {
			m1 = cur[0]
		}

		m2 := v + cur[0]
		cur[0], cur[1] = m1, m2
	}
	if cur[0] > cur[1] {
		return cur[0]
	}
	return cur[1]
}
```

