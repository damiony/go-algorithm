### 题目描述

**三个数的最大乘积**

给定一个整型数组，在数组中找出由三个数组成的最大乘积，并且返回。

**示例**

```shell
输入：[1, 2, 3, 4]
输出：24
```

### 解法

**思想：**

需要考虑两个负数和一个整数相乘的情况。所以计算最大的三个数的乘积，以及最小的两个数
和最大一个数的乘积，比较两者即可。

**解法一：** 排序

- 时间复杂度：`O(nlogn)`
- 空间复杂度：`O(1)`

```go
func maximumProduct(nums []int) int {
	sort.Ints(nums)
	n := len(nums)
	head := nums[0] * nums[1] * nums[n-1]
	tail := nums[n-1] * nums[n-2] * nums[n-3]
	if head > tail {
		return head
	}
	return tail
}
```

**解法二：** 线性遍历

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maximumProduct(nums []int) int {
	max1, max2, max3 := math.MinInt64, math.MinInt64, math.MinInt64
	min1, min2 := math.MaxInt64, math.MaxInt64
	for _, v := range nums {
		if max1 < v {
			max1, max2, max3 = v, max1, max2
		} else if max2 < v {
			max2, max3 = v, max2
		} else if max3 < v {
			max3 = v
		}

		if min1 > v {
			min1, min2 = v, min1
		} else if min2 > v {
			min2 = v
		}
	}

	one := max1 * max2 * max3
	two := max1 * min1 * min2
	if one < two {
		return two
	}
	return one
}
```