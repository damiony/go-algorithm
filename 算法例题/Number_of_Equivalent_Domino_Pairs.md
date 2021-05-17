### 题目描述

**等价多米诺骨牌对的数量：**

给定由一些多米诺骨牌组成的列表`dominoes`。如果`dominoes[i] = [a, b]`和`dominoes[j] = [c, d]`等价的前提是`a==c`且`b==d`，或是`a==d`且`b==c`。

在`0 <= i < j < dominoes.length`的前提下，找出满足`dominoes[i]`和`dominoes[j]`等价的骨牌对`(i, j)`的数量。

**示例：**

```shell
输入：dominoes = [[1,2], [2,1], [3,4], [5,6]]
输出：1
```

### 解法

**解法一：** 数组

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func numEquivDominoPairs(dominoes [][]int) int {
	edges := [10][10]int{}
    ans := 0
	for _, v := range dominoes {
		x, y := v[0], v[1]
		if x > y {
			x, y = y, x
		}
        ans += edges[x][y]
		edges[x][y]++
	}
	return ans
}
```