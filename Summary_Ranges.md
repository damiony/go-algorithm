### 题目描述

**汇总区间：**

给定一个无重复元素的有序整数数组 nums 。

返回**恰好覆盖数组中所有数字**的**最小有序**区间范围列表。也就是说，nums 的每个元素都恰好被某个区间范围所覆盖，并且不存在属于某个范围但不属于 nums 的数字 x 。

列表中的每个区间范围 [a,b] 应该按如下格式输出：

- "a->b" ，如果 a != b
- "a" ，如果 a == b

**示例：**

```bash
输入：nums = [0, 1, 2, 4, 5, 7]
输出：["0->2", "4->5", "7"]
```

### 解法

**解法一：** 双指针

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func summaryRanges(nums []int) []string {
	n := len(nums)
	var ans []string
	for fast := 0; fast < n; fast++ {
		slow := fast
		for fast+1 < n && nums[fast]+1 == nums[fast+1] {
			fast++
		}
		s := strconv.Itoa(nums[slow])
		if slow != fast {
			s += "->" + strconv.Itoa(nums[fast])
		}
		ans = append(ans, s)
	}
	return ans
}
```

