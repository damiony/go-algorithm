打家劫舍2

----

### 题目描述

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，能够偷窃到的最高金额。

示例：

```bash
输入：nums = [1,2,3,4]
输出：4
```

----

### 解法

解法一：回溯法

复杂度较高，不做讨论。

解法二：动态规划

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func rob(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}
	m1 := myrob(nums[0 : len(nums)-1])
	m2 := myrob(nums[1:])

	if m1 > m2 {
		return m1
	}
	return m2
}

func myrob(nums []int) int {
	pre, cur := 0, 0
	for i := 0; i < len(nums); i++ {
		tmp := cur
		if cur < pre+nums[i] {
			cur = pre + nums[i]
		}
		pre = tmp
	}
	return cur
}
```

