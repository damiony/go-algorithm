### 题目描述

**最大子序和：**

给定一个整数数组`nums`，找到一个具有最大和的连续子数组，要求子数组最少包含一个元素，并且返回最大和。

**示例：**

```shell
输入：[-2, -1, -3, 4, -1, 2, 1, -5, 4]
输出：6
```

### 解法

**解法一：暴力法**

时间复杂度过高，不讨论。

**解法二：常规的动态规划**

这是常规的动态规划解法，时间复杂度和空间复杂度都比较高。

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(n)`

```go
func maxSubArray(nums []int) int {
	max := math.MinInt64
	dp := make([]int, len(nums))
	for i := 0; i < len(nums); i++ {
		dp[i] = nums[i]
		if max < dp[i] {
			max = dp[i]
		}
	}
	for i := 2; i <= len(nums); i++ {
		for j := len(nums) - 1; j >= i-1; j-- {
			dp[j] = dp[j-1] + nums[j]
			if dp[j] > max {
				max = dp[j]
			}
		}
	}
	return max
}
```

**解法三：优化的动态规划**

思想：计算以`i`元素结尾的数组中，具有最大和的连续数组。`0 <= i <= n-1`

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxSubArray(nums []int) int {
	max := nums[0]
	pre := nums[0]
	for i := 1; i < len(nums); i++ {
		if pre+nums[i] > nums[i] {
			pre += nums[i]
		} else {
			pre = nums[i]
		}
		if pre > max {
			max = pre
		}
	}
	return max
}
```

**解法四：分治法**

解法很巧妙，值得仔细推敲。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(logn)`

```go
func maxSubArray(nums []int) int {
    return getLine(nums, 0, len(nums) - 1).sSum
}

type Line struct {
    // lSum 以左端点为边界的最长子序列和
    // rSum 以右端点为边界的最长子序列和
    // sSum 区间内的最长子序列和
    // iSum 区间序列和
    lSum, rSum, sSum, iSum int
}

func getLine(nums []int, l, r int) *Line {
    if l == r {
        return &Line{nums[l], nums[l], nums[l], nums[l]}
    }
    mid := l + (r - l) >> 1
    left := getLine(nums, l, mid)
    right := getLine(nums, mid + 1, r)

    return pushUp(left, right)
}

func pushUp(left, right *Line) *Line {
    lSum := left.lSum
    if lSum < left.iSum + right.lSum {
        lSum = left.iSum + right.lSum
    }

    rSum := right.rSum
    if rSum < right.iSum + left.rSum {
        rSum = right.iSum + left.rSum
    }

    sSum := left.sSum
    if sSum < right.sSum {
        sSum = right.sSum
    }
    if sSum < left.rSum + right.lSum {
        sSum = left.rSum + right.lSum
    }

    iSum := left.iSum + right.iSum
    return &Line{lSum, rSum, sSum, iSum}
}
```
