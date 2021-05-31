### 题目描述

**零钱兑换**

给定不同面额硬币`coins`和一个总额度`amount`，计算凑成总额数所需的最少硬币个数，如果无法凑成总额数，返回`-1`。

**示例：**

```shell
输入：coins = [1, 2, 5], amount = 11
输出：3
```

### 解法

**这个题型很经典，大部分动态规划的题目都能从其中找到解题思路**

解法一：贪心算法

- 时间复杂度：待定
- 空间复杂度：待定

```go
func coinChange(coins []int, amount int) int {
    ans := amount + 1
    var dfs func(coins []int, amount int, cnt int)
    dfs = func(coins []int, amount int, cnt int) {
        if len(coins) == 0 {
            return
        }
        for i := amount/coins[0]; i >= 0; i-- {
            if cnt + i > ans {
                break
            }
            if amount - i*coins[0] == 0 {
                if ans > cnt+i {
                    ans = cnt+i
                }
                break
            }
            dfs(coins[1:], amount-i*coins[0], cnt+i)
        }
    }

    sort.Slice(coins, func (i, j int) bool { return coins[i] > coins[j] })
    dfs(coins, amount, 0)
    if ans == amount + 1 {
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
    if amount == 0 {
        return 0
    }
    ans := 0
    queue := []int{amount} // 树状按层搜索
    vis := make(map[int]bool, amount) // 记忆
    for len(queue) > 0 {
        qLen := len(queue)
        ans++
        for i := 0; i < qLen; i++ {
            num := queue[i]
            for _, coin := range coins {
                if vis[num-coin] { // 剪枝
                    continue
                }
                if num - coin < 0 { // 剪枝
                    continue
                }
                if num - coin == 0 { // 搜索结束
                    return ans
                }
                vis[num-coin] = true
                queue = append(queue, num-coin)
            }
        }
        queue = queue[qLen:]
    }
    return -1
}
```

解法三：动态规划（自顶向下）

- 时间复杂度：`O(amount * n)`
- 空间复杂度：`O(amount)`

```go
func coinChange(coins []int, amount int) int {
    // cache[i] 表示凑足金额i需要的最少个数
    cache := make([]int, amount+1)
    return dfs(coins, amount, cache)
}

func dfs(coins []int, amount int, cache []int) int {
    if amount == 0 {
        return 0
    }
    if cache[amount] > 0 || cache[amount] == -1 {
        return cache[amount]
    }

    // dp[i] = min{dp[i-coin]} + 1
    // or dp[i] = -1
    ans := amount+1
    for _, coin := range coins {
        if amount - coin < 0 {
            continue
        }
        cnt := dfs(coins, amount-coin, cache)
        if cnt >= 0 && ans > cnt+1 {
            ans = cnt+1
        }
    }
    if ans == amount+1 {
        ans = -1
    }

    cache[amount] = ans
    return cache[amount]
}
```

解法四：动态规划（自下向上）

- 时间复杂度：`O(amount * n)`
- 空间复杂度：`O(amount)`

```go
func coinChange(coins []int, amount int) int {
    // dp[i] 表示凑足金额i需要的最少个数
    // dp[i] = min{dp[i-coin]} + 1
    // or dp[i] = -1
    dp := make([]int, amount+1)
    dp[0] = 0
    for i := 1; i <= amount; i++ {
        dp[i] = i+1
        for _, coin := range coins {
            if i - coin >= 0 && dp[i-coin] != -1 && dp[i] > dp[i-coin]+1 {
                dp[i] = dp[i-coin] + 1
            }
        }
        if dp[i] == i+1 {
            dp[i] = -1
        }
    }
    return dp[amount]
}
```

