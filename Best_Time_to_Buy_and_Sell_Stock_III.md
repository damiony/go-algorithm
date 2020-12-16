买卖股票的最佳时机3

----

### 题目描述

给定一个数组，它的第`i`个元素是一支给定股票在第`i`天的价格。如果最多只能完成两笔交易，求能获得的最大利润。

示例：

```bash
输入：[3, 3, 5, 0, 0, 3, 1, 4]
输出：6
```

----

### 解法

解法一：回溯递归

- 时间复杂度：`O(2^n)`
- 空间复杂度：`O(logn)`

```go
func maxProfit(prices []int) int {
	status, index, count, profit, max := 0, 0, 0, 0, 0
	return dfs(prices, status, index, count, profit, max)
}

func dfs(prices []int, status, index, count, profit, max int) int {
	if index == len(prices) || count == 2 {
		if max < profit {
			max = profit
		}
		return max
	}

	if status == 1 { // 持有
		max = dfs(prices, 1, index+1, count, profit, max)
		max = dfs(prices, 0, index+1, count+1, profit+prices[index], max)
	} else { // 不持有
		max = dfs(prices, 1, index+1, count, profit-prices[index], max)
		max = dfs(prices, 0, index+1, count, profit, max)
	}
	return max
}
```

解法二：回溯递归 + 备忘录

- 时间复杂度：`O(n)`
- 空间复杂度：`O(logn)`

```go

func maxProfit(prices []int) int {
	mem := make([][][]int, len(prices))
	for i := range mem {
		mem[i] = make([][]int, 2)
		for j := range mem[i] {
			mem[i][j] = []int{-1, -1}
		}
	}

	return dfs(prices, mem, 0, 0, 0, 0)
}

func dfs(prices []int, mem [][][]int, status, index, profit, count int) int {
	if index == len(prices) || count == 2 {
		return profit
	}
	if mem[index][status][count] >= 0 {
		return mem[index][status][count]
	}
	p1, p2 := 0, 0
	if status == 1 {
		p1 = dfs(prices, mem, 1, index+1, profit, count)
		p2 = dfs(prices, mem, 0, index+1, profit, count+1) + prices[index]
	} else {
		p1 = dfs(prices, mem, 1, index+1, profit, count) - prices[index]
		p2 = dfs(prices, mem, 0, index+1, profit, count)
	}
	maxP := p1
	if maxP < p2 {
		maxP = p2
	}

	mem[index][status][count] = maxP
	return mem[index][status][count]
}
```

解法三：两次循环

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go

func maxProfit(prices []int) int {
	n := len(prices)
	if n <= 0 {
		return 0
	}

	min := prices[0]
	dp := make([]int, n)
	dp[0] = 0
	for i := 1; i < n; i++ {
		if min > prices[i] {
			min = prices[i]
		}
		dp[i] = dp[i-1]
		if dp[i] < prices[i]-min {
			dp[i] = prices[i] - min
		}
	}

	ans := dp[n-1]
	max := 0
	pre := 0
	for i := n - 1; i > 0; i-- {
		if max < prices[i] {
			max = prices[i]
		}
		if pre < max-prices[i] {
			pre = max - prices[i]
		}
		if ans < pre+dp[i-1] {
			ans = pre + dp[i-1]
		}
	}
	return ans
}
```

解法四：动态规划

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	// 每阶段状态
	// 0 买卖0次 当前未持有
	// 1 买卖0次 当前持有
	// 2 买卖1次 当前未持有
	// 3 买卖1次 当前持有
	// 4 买卖2次 当前未持有
	dp := []int{0, -prices[0], 0, -prices[0], 0}
	for i := 1; i < len(prices); i++ {
		dp[4] = getmax(dp[4], dp[3]+prices[i])
		dp[3] = getmax(dp[3], dp[2]-prices[i])
		dp[2] = getmax(dp[2], dp[1]+prices[i])
		dp[1] = getmax(dp[1], dp[0]-prices[i])
	}

	ans := 0
	for _, v := range dp {
		if ans < v {
			ans = v
		}
	}

	return ans
}

func getmax(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

解法五：单次循环

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	if len(prices) <= 0 {
		return 0
	}

	buy1, sell1, buy2, sell2 := prices[0], 0, prices[0], 0
	for i := 0; i < len(prices); i++ {
		if buy1 > prices[i] {
			buy1 = prices[i]
		}
		if sell1 < prices[i]-buy1 {
			sell1 = prices[i] - buy1
		}
		if buy2 > prices[i]-sell1 {
			buy2 = prices[i] - sell1
		}
		if sell2 < prices[i]-buy2 {
			sell2 = prices[i] - buy2
		}
	}
	return sell2
}
```

