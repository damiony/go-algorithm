### 题目描述

**买卖股票的最佳时机：**

给定一个数组，它的第`i`个元素是一支给定股票第`i`天的价格。

如果最多只允许完成一笔交易，求能获取的最大利润。

**示例：**

```bash
输入：[7, 1, 5, 3, 6, 4]
输出：5
```

### 解法

**解法一：暴力求解**

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	max := 0
	for i, v := range prices {
		cur := -v
		for j := i+1; j < len(prices); j++ {
			if  prices[j] - v > cur {
				cur = prices[j] - v
			}
		}
		if max < cur {
			max = cur
		}
	}
	return max
}
```

**解法二：单次循环**

计算当前值与最小值得差，取差里面得最小值。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	ans, min := 0, math.MaxInt64
	for i := 0; i < len(prices); i++ {
		if prices[i] < min {
			min = prices[i]
		}
		if ans < prices[i]-min {
			ans = prices[i] - min
		}
	}
	return ans
}
```

**解法三：单调栈**

利用了当前值与最小值之差的思想。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxProfit(prices []int) int {
	ans := 0
	stack := []int{}
	for i := 0; i < len(prices); i++ {
		for len(stack) > 0 && stack[len(stack)-1] > prices[i] {
			stack = stack[:len(stack)-1]
		}
		stack = append(stack, prices[i])
		if len(stack) == 1 {
			continue
		}
		if ans < prices[i]-stack[0] {
			ans = prices[i] - stack[0]
		}
	}
	return ans
}
```

**解法四：动态规划1**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxProfit(prices []int) int {
	if len(prices) == 0 {
		return 0
	}
	// dp[i] = max(dp[i-1], prices[i] - minPrice)
	// dp优化成pre
	pre, min := 0, prices[0]
	for i := 1; i < len(prices); i++ {
		if min > prices[i] {
			min = prices[i]
		}
		if pre < prices[i] - min {
			pre = prices[i] - min
		}
	}
	return pre
}
```

**解法五：动态规划2**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxProfit(prices []int) int {
	if len(prices) == 0 {
		return 0
	}

	// dp[i] = max(0, dp[i-1] + prices[i] - prices[i-1])
	ans := 0
	dp := make([]int, len(prices))
	dp[0] = 0
	for i := 1; i < len(prices); i++ {
		if dp[i-1]+prices[i]-prices[i-1] > 0 {
			dp[i] = dp[i-1] + prices[i] - prices[i-1]
		}
		if dp[i] > ans {
			ans = dp[i]
		}
	}
	return ans
}
```

**解法六：动态规划3**

是上面解法四的优化解法。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	if len(prices) == 0 {
		return 0
	}

	// dp[i] = max(0, dp[i-1] + prices[i] - prices[i-1])
	// 将dp[i]优化成pre
	ans, pre := 0, 0
	for i := 1; i < len(prices); i++ {
		if pre+prices[i]-prices[i-1] > 0 {
			pre += prices[i] - prices[i-1]
		} else {
			pre = 0
		}
		if pre > ans {
			ans = pre
		}
	}
	return ans
}
```

**解法六：动态规划4**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func maxProfit(prices []int) int {
	if len(prices) == 0 {
		return 0
	}
	dp := []int{-prices[0], 0} // 持有 未持有
	for i := 1; i < len(prices); i++ {
		if dp[1] < dp[0]+prices[i] {
			dp[1] = dp[0] + prices[i]
		}
		if dp[0] < -prices[0] {
			dp[0] = -prices[0]
		}
	}
	return dp[1]
}
```