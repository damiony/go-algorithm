买卖股票的最佳时机4

----

给定一个整数数组`prices`，它的第`i`个元素是一支给定股票在第`i`天的价格。

要求最多可以完成`k`笔交易，求能获得的最大利润。

示例：

```bash
输入：k = 2, prices = [2, 4, 1]
输出：2
```

----

### 解法

解法一：暴力法

代码就是普通的回溯写法，具体代码可以参考“买卖股票的其它题型”，不再详谈。

解法二：回溯 + 备忘录 

给回溯算法加上备忘录，具体代码可以参考“买卖股票的其它题型”，不再详谈。

解法三：动态规划

和“最多买卖两次股票”的题型类似，代码不再多说。

解法四：一次循环

代码其实同样和“最多两次买卖股票”的题型类似，代码简单，所以说一下。

- 时间复杂度：`O(k * n)`
- 空间复杂度：`O(k)`

```go
func maxProfit(k int, prices []int) int {
	if len(prices) <= 0 {
		return 0
	}
    if k <= 0 {
        return 0
    }

	mem := make([][]int, k)
	for i := range mem {
		mem[i] = []int{prices[0], 0}
	}
	for i := 0; i < len(prices); i++ {
		if mem[0][0] > prices[i] {
			mem[0][0] = prices[i]
		}
		for j := range mem {
			if j != 0 {
				if mem[j][0] > prices[i]-mem[j-1][1] {
					mem[j][0] = prices[i] - mem[j-1][1]
				}
				if mem[j][1] < prices[i]-mem[j][0] {
					mem[j][1] = prices[i] - mem[j][0]
				}
			} else {
				if mem[j][1] < prices[i]-mem[j][0] {
					mem[j][1] = prices[i] - mem[j][0]
				}
			}
		}
	}
	return mem[k-1][1]
}
```

