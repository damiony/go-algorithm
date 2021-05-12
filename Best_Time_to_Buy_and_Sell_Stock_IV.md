### 题目描述

**买卖股票的最佳时机4**

给定一个整数数组`prices`，它的第`i`个元素是一支给定股票在第`i`天的价格。

要求最多可以完成`k`笔交易，求能获得的最大利润。

**示例：**

```shell
输入：k = 2, prices = [2, 4, 1]
输出：2
```

### 解法

**解法一：暴力法**

代码就是普通的回溯写法，具体代码可以参考“买卖股票的其它题型”，不再详谈。

**解法二：回溯 + 备忘录**

给回溯算法加上备忘录，具体代码可以参考“买卖股票的其它题型”，不再详谈。

**解法三：动态规划**

- 时间复杂度：`O(k * n)`
- 空间复杂度：`O(k)`

```go
func maxProfit(k int, prices []int) int {
    if k == 0 || len(prices) == 0 {
        return 0
    }

    // 奇数下标 表示不持有股票
    // 偶数下标 表示持有股票
    dp := make([]int, 2*k)
    for i := 0; i < len(dp); i += 2 {
        dp[i] = -prices[0]
    }

    for i := 1; i < len(prices); i++ {
        for j := len(dp)-1; j >= 1; j-- {
            if j & 1 == 1 {
                if dp[j] < dp[j-1] + prices[i] {
                    dp[j] = dp[j-1] + prices[i]
                }
            } else {
                if dp[j] < dp[j-1] - prices[i] {
                    dp[j] = dp[j-1] - prices[i]
                }
            }
        }
        if dp[0] < -prices[i] {
            dp[0] = -prices[i]
        }
    }

    return dp[len(dp)-1]
}
```
