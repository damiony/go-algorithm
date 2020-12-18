买卖股票的最佳时机（含手续费）

----

### 题目描述

给定一个整数数组`prices`，其中第`i`个元素代表了第`i`天的股票价格，非负整数`fee`代表了交易股票的手续费用。

假设买入股票之后，必须卖出才能再次买入，且每次交易都需要付手续费，求利润最大值。

示例：

```bash
输入：prices = [1, 3, 2, 8, 4, 9], fee = 2
输出：8
```

----

### 解法

解法一：动态规划

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int, fee int) int {
	if len(prices) <= 0 {
		return 0
	}
	dp := []int{0, -prices[0]}
	for i := 1; i < len(prices); i++ {
		tmp := dp[0]
		if dp[0] < dp[1]+prices[i]-fee {
			dp[0] = dp[1] + prices[i] - fee
		}
		if dp[1] < tmp-prices[i] {
			dp[1] = tmp - prices[i]
		}
	}
	return dp[0]
}
```

解法二：贪心算法

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int, fee int) int {
	if len(prices) <= 1 {
		return 0
	}
	ans, min := 0, prices[0]
	for i := 0; i < len(prices); i++ {
		if prices[i] < min {
			min = prices[i]
		} else {
			if prices[i]-fee > min {
				ans += prices[i] - fee - min
				min = prices[i] - fee
			}
		}
	}
	return ans
}
```

