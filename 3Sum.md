### 题目描述

**三数之和：**

给你一个包含`n`个整数的数组`nums`，判断`nums`中是否存在三个元素`a, b, c,`，使得`a + b + c = 0`？请你找出所有满足条件且不重复的三元组。

注：不可包含重复的三元数组。

示例：
```shell
输入：[-1,0,1,2,-1,-4]
输出：
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

### 解法

**方法一：** 暴力求解(**不推荐**)

先排序并且遍历一遍数组，记录每个元素及其下标。然后循环遍历两遍数组，找出符合条件的组合，然后输出。

- 时间复杂度：O(n^2)
- 空间复杂度: O(n)

```go
func threeSum(nums []int) [][]int {
	sort.Ints(nums)

	length := len(nums)
	if length == 0 || nums[length-1] < 0 {
		return nil
	}

	nMap := map[int]int{}
	for i := 0; i < length; i++ {
		nMap[nums[i]] = i
	}

	ans := [][]int{}
	for i := 0; i < length-2; i++ {
		if i > 0 && nums[i] == nums[i-1] {
			continue
		}
		for j := i + 1; j < length-1; j++ {
			if j > i+1 && nums[j] == nums[j-1] {
				continue
			}
			target := -nums[i] - nums[j]
			if idx, ok := nMap[target]; ok && idx > j {
				ans = append(ans, []int{nums[i], nums[j], target})
			}
		}
	}

	return ans
}
```

**方法二：** 排序 + 两次遍历 + 双指针

- 时间复杂度: O(n^2)
- 空间复杂度: O(1)

```go
func threeSum(nums []int) [][]int {
	sort.Ints(nums)
	result := [][]int{}
	length := len(nums)

	for i := 0; i < len(nums)-2; i++ {
		if i > 0 && nums[i] == nums[i-1] {
			continue
		}
		if nums[i] > 0 || nums[length-1] < 0 {
			break
		}

		left := i + 1
		right := len(nums) - 1
		for left < right {
			if left > i+1 && nums[left] == nums[left-1] {
				left++
				continue
			}
			if right < length-1 && nums[right] == nums[right+1] {
				right--
				continue
			}

			if nums[i]+nums[left]+nums[right] > 0 {
				right--
			} else if nums[i]+nums[left]+nums[right] < 0 {
				left++
			} else {
				group := []int{nums[i], nums[left], nums[right]}
				result = append(result, group)
				right--
			}
		}
	}
	return result
}
```