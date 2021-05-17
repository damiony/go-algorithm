最佳买卖股票时机（含冷冻期）

----

### 题目描述

给定一个整数数组，其中第`i`个元素代表了第`i`天的股票价格。

约束条件为：

- 必须在再次购买前出售掉之前的股票。
- 卖出股票后，无法在第二天买入股票，即冷冻期为1天。

求能获得的最大利润。

示例：

```bash
输入：[1, 2, 3, 0, 2]
输出：3
```

----

### 解法

解法一：动态规划

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	if len(prices) <= 0 {
		return 0
	}
	dp := []int{0, -prices[0], 0}
	for i := 1; i < len(prices); i++ {
		tmp0, tmp1 := dp[0], dp[1]
		if dp[0] < dp[2] {
			dp[0] = dp[2]
		}
		if dp[1] < tmp0-prices[i] {
			dp[1] = tmp0 - prices[i]
		}
		dp[2] = tmp1 + prices[i]
	}

	ans := dp[0]
	if ans < dp[2] {
		ans = dp[2]
	}
	return ans
}
```

