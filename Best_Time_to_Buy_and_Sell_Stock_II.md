买卖股票的最佳时机2

----

### 题目描述

给定一个数组，它的第`i`个元素是一支给定股票第`i`天的价格。

设计一个算法来计算所能获得的最大利润。可以尽可能完成更多交易。

示例：

```bash
输入：[7, 1, 5, 3, 6, 4]
输出：7
```

----

### 解法

解法一：贪心算法

- 时间复杂度：`(O)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
    count := 0
    for i := 1; i < len(prices); i++ {
        if prices[i-1] < prices[i] {
            count += prices[i] - prices[i-1]
        }
    }
    return count
}
```

解法二：动态规划

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n^2)`

```go
func maxProfit(prices []int) int {
	n := len(prices)
	dp := make([][]int, n)
	for i := range dp {
		dp[i] = make([]int, 2)
	}

	dp[0][0] = 0
	dp[0][1] = -prices[0]

	for i := 1; i < len(prices); i++ {
		dp[i][0] = dp[i-1][0]
		if dp[i][0] < dp[i-1][1]+prices[i] {
			dp[i][0] = dp[i-1][1] + prices[i]
		}

		dp[i][1] = dp[i-1][1]
		if dp[i][1] < dp[i-1][0]-prices[i] {
			dp[i][1] = dp[i-1][0] - prices[i]
		}
	}

	return dp[len(prices)-1][0]
}
```

解法三：动态规划

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	dp := make([]int, 2)
	dp[0] = 0
	dp[1] = -prices[0]

	for i := 1; i < len(prices); i++ {
		status0, status1 := dp[0], dp[1]
		if dp[1] < status0-prices[i] {
			dp[1] = status0 - prices[i]
		}

		if dp[0] < status1+prices[i] {
			dp[0] = status1 + prices[i]
		}
	}

	return dp[0]
}
```

