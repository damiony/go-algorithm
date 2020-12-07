岛屿数量

----

### 题目描述

给定 '1'（陆地） 和 '0'（水） 组成的二维网格，计算网络中岛屿的数量。

岛屿总是被水包围，并且岛屿只能由水平和竖直方向的相邻陆地连接形成。

示例：

```bash
输入：
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

输出：3
```

----

### 解法

解法一：深度优先遍历

- 时间复杂度：`O(M*N)`
- 空间复杂度：`O(M*N)`

```go
func numIslands(grid [][]byte) int {
	nr := len(grid)
	nc := len(grid[0])
	numsOfLands := 0
	for r := 0; r < nr; r++ {
		for c := 0; c < nc; c++ {
			if grid[r][c] == '1' {
				dfs(grid, r, c)
				numsOfLands++
			}
		}
	}
	return numsOfLands
}

func dfs(grid [][]byte, r, c int) {
	nr := len(grid)
	nc := len(grid[0])
	if r < 0 || c < 0 || r >= nr || c >= nc || grid[r][c] == '0' {
		return
	}

	grid[r][c] = '0'
	dfs(grid, r+1, c)
	dfs(grid, r-1, c)
	dfs(grid, r, c+1)
	dfs(grid, r, c-1)
}
```



解法二：广度优先遍历

- 时间复杂度：`O(M*N)`
- 空间复杂度：`O(min(M, N))`

```go
func numIslands(grid [][]byte) int {
	nr := len(grid)
	nc := len(grid[0])
	numsOfLands := 0

	for r := 0; r < nr; r++ {
		for c := 0; c < nc; c++ {
			if grid[r][c] == '1' {
				numsOfLands++
				queue := [][]int{}
				queue = append(queue, []int{r, c})
				grid[r][c] = '0'
				for len(queue) > 0 {
					pos := queue[0]
					queue = queue[1:]
					i, j := pos[0], pos[1]
					if i+1 < nr && grid[i+1][j] == '1' {
						queue = append(queue, []int{i + 1, j})
					    grid[i+1][j] = '0'
					}
					if i-1 >= 0 && grid[i-1][j] == '1' {
						queue = append(queue, []int{i - 1, j})
					    grid[i-1][j] = '0'
					}
					if j+1 < nc && grid[i][j+1] == '1' {
						queue = append(queue, []int{i, j + 1})
					    grid[i][j+1] = '0'
					}
					if j-1 >= 0 && grid[i][j-1] == '1' {
						queue = append(queue, []int{i, j - 1})
					    grid[i][j-1] = '0'
					}
				}
			}
		}
	}
	return numsOfLands
}
```



解法三：并查集

- 时间复杂度：
- 空间复杂度：`O(M*N)`

```go
func numIslands(grid [][]byte) int {
	nr := len(grid)
	nc := len(grid[0])
	u := &unionFind{
		Count:  0,
		Parent: make([]int, nr*nc),
		Rank:   make([]int, nr*nc),
	}
	// 初始化并查集
	for r := 0; r < nr; r++ {
		for c := 0; c < nc; c++ {
			if grid[r][c] == '1' {
				u.makeSet(r*nc + c)
			}
		}
	}

	// 合并并查集
	for r := 0; r < nr; r++ {
		for c := 0; c < nc; c++ {
			if grid[r][c] == '1' {
				grid[r][c] = '0'
				if r+1 < nr && grid[r+1][c] == '1' {
					u.union(r*nc+c, (r+1)*nc+c)
				}
				if r-1 >= 0 && grid[r-1][c] == '1' {
					u.union(r*nc+c, (r-1)*nc+c)
				}
				if c+1 < nc && grid[r][c+1] == '1' {
					u.union(r*nc+c, r*nc+c+1)
				}
				if c-1 >= 0 && grid[r][c-1] == '1' {
					u.union(r*nc+c, r*nc+c-1)
				}
			}
		}
	}

	return u.getCounts()
}

/*
 * 以下是并查集的相关定义
 */
type unionFind struct {
	Count  int   // 集合个数
	Parent []int // 对应祖先
	Rank   []int // 秩
}

func (u *unionFind) makeSet(x int) {
	u.Parent[x] = x
	u.Rank[x] = 0
	u.Count++
}

func (u *unionFind) find(x int) int {
	if u.Parent[x] != x {
		u.Parent[x] = u.find(u.Parent[x])
	}

	return u.Parent[x]
}

func (u *unionFind) union(x, y int) {
	xRoot := u.find(x)
	yRoot := u.find(y)
	if xRoot == yRoot {
		return
	}

	if u.Rank[xRoot] < u.Rank[yRoot] {
		u.Parent[xRoot] = yRoot
	} else if u.Rank[xRoot] > u.Rank[yRoot] {
		u.Parent[yRoot] = xRoot
	} else {
		u.Parent[yRoot] = xRoot
		u.Rank[xRoot]++
	}
	u.Count--
	return
}

func (u *unionFind) getCounts() int {
	return u.Count
}
```

