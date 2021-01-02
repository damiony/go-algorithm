买卖股票的最佳时机2

----

### 题目描述

给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润，必须在再次购买前出售掉之前的股票。

示例：

```bash
输入：[7, 1, 5, 3, 6, 4]
输出：7
```

----

### 解法：

解法一：贪心算法

贪心算法本质为：求不相交的若干区间，使其差值之和最大，此处区间长度不一定为1。

可以将这种情况等价为：求若干长度为1的区间，使其差值最大。只要区间的差值为正，就可以放入累计的结果中。

- 时间复杂度：`O(N)`
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



解法二：暴力回溯（时间复杂度很高）

- 时间复杂度：`O(2^N)`
- 空间复杂度：`O(N)`

```go
func maxProfit(prices []int) int {
	maxPrice := math.MinInt64
	maxPrice = dfs(prices, 0, 0, 0, maxPrice)
	return maxPrice
}

func dfs(prices []int, layer, status, total, maxPrice int) int {
	if layer >= len(prices) {
		if total > maxPrice {
			maxPrice = total
		}
		return maxPrice
	}

	maxPrice = dfs(prices, layer+1, status, total, maxPrice)

	if status == 1 {
		maxPrice = dfs(prices, layer+1, 0, total+prices[layer], maxPrice)
	} else {
		maxPrice = dfs(prices, layer+1, 1, total-prices[layer], maxPrice)
	}

	return maxPrice
}
```



解法三：动态规划

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

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



解法四：优化的动态规划

- 时间复杂度：`O(N)`
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

