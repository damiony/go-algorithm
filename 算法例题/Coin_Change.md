零钱兑换

----

### 题目描述

给定不同面额硬币`coins`和一个总额度`amount`，计算凑成总额数所需的最少硬币个数，如果无法凑成总额数，返回`-1`。

示例：

```bash
输入：coins = [1, 2, 5], amount = 11
输出：3
```

----

### 解法

**这个题型很经典，大部分动态规划的题目都能从其中找到解题思路**

解法一：贪心算法

- 时间复杂度：待定
- 空间复杂度：待定

```go
func coinChange(coins []int, amount int) int {
	ans := amount + 1
	var dfs func(coins []int, index int, amount int, cnt int)
	dfs = func(coins []int, index int, amount int, cnt int) {
		if index < 0 {
			return
		}
		for i := amount / coins[index]; i >= 0; i-- {
			if i+cnt > ans {
				break
			}
			if amount-i*coins[index] == 0 {
				if ans > cnt+i {
					ans = cnt + i
				}
				break
			}
			dfs(coins, index-1, amount-i*coins[index], cnt+i)
		}
		return
	}

	sort.Ints(coins)
	dfs(coins, len(coins)-1, amount, 0)
	if ans == amount+1 {
		ans = -1
	}
	return ans
}
```

解法二：迭代法

- 时间复杂度：`O(amount * n)`
- 空间复杂度：`O(amount)`

```go

func coinChange(coins []int, amount int) int {
	if amount <= 0 {
		return 0
	}

	queue := []int{amount}
	history := make([]bool, amount+1)
	history[amount] = true
	ans := 0
	for len(queue) > 0 {
		q := queue
		queue = nil
		ans++
		for _, num := range q {
			for _, v := range coins {
				if num-v == 0 {
					return ans
				}
				if num-v < 0 {
					continue
				}
				if history[num-v] {
					continue
				}
				history[num-v] = true
				queue = append(queue, num-v)
			}
		}
	}
	return -1
}
```

解法三：动态规划（自顶向下）

- 时间复杂度：`O(amount * n)`
- 空间复杂度：`O(amount)`

```go
func coinChange(coins []int, amount int) int {
	if amount <= 0 {
		return 0
	}
	mem := make([]int, amount+1)
	return dfs(coins, amount, mem)
}

func dfs(coins []int, amount int, mem []int) int {
	if amount <= 0 {
		return 0
	}
	if mem[amount] != 0 {
		return mem[amount]
	}

	min := amount + 1
	for _, v := range coins {
		if amount-v < 0 {
			continue
		}
		cnt := dfs(coins, amount-v, mem)
		if cnt >= 0 && cnt+1 < min {
			min = cnt + 1
		}
	}

	if min == amount+1 {
		min = -1
	}
	mem[amount] = min
	return mem[amount]
}
```

解法四：动态规划（自下向上）

- 时间复杂度：`O(amount * n)`
- 空间复杂度：`O(amount)`

```go
func coinChange(coins []int, amount int) int {
	if amount <= 0 {
		return 0
	}

	dp := make([]int, amount+1)
	dp[0] = 0
	for i := 1; i <= amount; i++ {
		dp[i] = amount + 1
		for _, v := range coins {
			if i-v < 0 || dp[i-v] == -1 {
				continue
			}
			if dp[i] > dp[i-v]+1 {
				dp[i] = dp[i-v] + 1
			}
		}
		if dp[i] == amount+1 {
			dp[i] = -1
		}
	}
	return dp[amount]
}
```

