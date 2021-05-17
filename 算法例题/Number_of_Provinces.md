### 题目描述

**省份数量：**

有`n`个城市，其中一些彼此相连，另一些没有相连。如果城市`a`与城市`b`直接相连，且城市`b`与城市`c`直接相连，那么城市`a`与城市`c`间接相连。

”省份“是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给定一个`n x n`的矩阵`isConnected`，其中`isConnected[i][j] = 1`表示第`i`个城市和第`j`个城市直接相连，而`isConnected[i][j] = 0`表示二者不直接相连。

返回矩阵中 省份 的数量。

**示例**

```bash
输入：isConnected = [[1,1,0],[1,1,0],[0,0,1]]
输出：2
```

### 解法

**解法一：**深度优先遍历

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(n^2)`

```go
func findCircleNum(isConnected [][]int) int {
	n, ans := len(isConnected), 0
	vis := make([]bool, n)
	for from := 0; from < n; from++ {
		if vis[from] {
			continue
		}
		ans++
		dfs(isConnected, vis, from)
	}
	return ans
}

func dfs(isConnected [][]int, vis []bool, from int) {
	vis[from] = true
	for to := 0; to < len(isConnected); to++ {
		if isConnected[from][to] == 1 && !vis[to] {
			dfs(isConnected, vis, to)
		}
	}
	return
}
```

**解法二：**广度优先遍历

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(n)`

```go
func findCircleNum(isConnected [][]int) int {
	n, ans := len(isConnected), 0
	vis := make([]bool, n)
	for city, v := range vis {
		if v {
			continue
		}
		ans++
		vis[city] = true
		queue := []int{city}
		for len(queue) > 0 {
			from := queue[0]
			queue = queue[1:]
			for to := 0; to < n; to++ {
				if !vis[to] && isConnected[from][to] == 1 {
					vis[to] = true
					queue = append(queue, to)
				}
			}
		}
	}
	return ans
}
```

**解法三：**并查集

- 时间复杂度：较复杂，小于`O(n^2)`
- 空间复杂度：`O(1)`

```go

func findCircleNum(isConnected [][]int) int {
	n := len(isConnected)
	father := make([]int, n)
	for i := range father {
		father[i] = i
	}

	var find func(int) int
	find = func(x int) int {
		if father[x] != x {
			father[x] = find(father[x])
		}
		return father[x]
	}
	union := func(x, y int) {
		father[find(x)] = find(y)
	}

	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if isConnected[i][j] == 1 {
				union(i, j)
			}
		}
	}

	var ans int
	for i, f := range father {
		if i == f {
			ans++
		}
	}
	return ans
}
```

