### 题目描述

**岛屿数量：**

给定 '1'（陆地） 和 '0'（水） 组成的二维网格，计算网络中岛屿的数量。

岛屿总是被水包围，并且岛屿只能由水平和竖直方向的相邻陆地连接形成。

**示例：**

```shell
输入：
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]

输出：3
```

### 解法

**解法一：深度优先遍历**

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

**解法二：广度优先遍历**

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
	r, c := len(grid), len(grid[0])
	uf := NewUnionFind(r * c)
	for i := range grid {
		for _, v := range grid[i] {
			if v == '1' {
				uf.count++
			}
		}
	}

	for i := range grid {
		for j := range grid[i] {
			if grid[i][j] == '0' {
				continue
			}
			if j+1 < c && grid[i][j+1] == '1' {
				uf.union(i*c+j, i*c+j+1)
			}
			if i+1 < r && grid[i+1][j] == '1' {
				uf.union(i*c+j, (i+1)*c+j)
			}
		}
	}

	return uf.count
}

type unionFind struct {
	father, rank []int
	count        int
}

func NewUnionFind(n int) *unionFind {
	father, rank := make([]int, n), make([]int, n)
	for i := range father {
		father[i] = i
		rank[i] = i
	}
	return &unionFind{father, rank, 0}
}

func (uf *unionFind) find(x int) int {
	if uf.father[x] != x {
		uf.father[x] = uf.find(uf.father[x])
	}
	return uf.father[x]
}

func (uf *unionFind) union(x, y int) {
	rootX, rootY := uf.find(x), uf.find(y)
	if rootX == rootY {
		return
	}

	if uf.rank[rootX] < uf.rank[rootY] {
		rootX, rootY = rootY, rootX
	}
	uf.father[rootY] = rootX
	uf.rank[rootX] += uf.rank[rootY]
	uf.count--
	return
}
```