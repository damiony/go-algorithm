不同路径2

----

### 题目描述

一个机器人位于`mxn`网格左上角，每次只能向下或者向右移动异步，机器人试图到达网格右下角。

在考虑障碍物的情况下，有多少条不同的路径？

示例：

```bash
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
```

----

### 解法

解法一：动态规划1

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
	r, c := len(obstacleGrid), len(obstacleGrid[0])
	dp := make([][]int, r)
	for i := range dp {
		dp[i] = make([]int, c)
	}

	for i := 0; i < r; i++ {
		if obstacleGrid[i][0] == 1 {
			break
		}
		dp[i][0] = 1
	}
	for i := 0; i < c; i++ {
		if obstacleGrid[0][i] == 1 {
			break
		}
		dp[0][i] = 1
	}

	for i := 1; i < r; i++ {
		for j := 1; j < c; j++ {
			if obstacleGrid[i][j] == 1 {
				continue
			}
			dp[i][j] = dp[i-1][j] + dp[i][j-1]
		}
	}
	return dp[r-1][c-1]
}
```

解法二：动态规划2

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(n)`，`n`代表网格列的长度

```go
func uniquePathsWithObstacles(obstacleGrid [][]int) int {
	r, c := len(obstacleGrid), len(obstacleGrid[0])

	dp := make([]int, c)
	for i := range dp {
		if obstacleGrid[0][i] == 1 {
			break
		}
		dp[i] = 1
	}

	for i := 1; i < r; i++ {
		for j := 0; j < c; j++ {
			if obstacleGrid[i][j] == 1 {
				dp[j] = 0
			} else if j != 0 {
				dp[j] = dp[j-1] + dp[j]
			}
		}
	}
	return dp[c-1]
}
```

