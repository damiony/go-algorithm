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
    n, m := len(grid), len(grid[0])
    uf := newUnionFindSet(n * m)

    counts := 0
    for i := range grid {
        for j := range grid[i] {
            if grid[i][j] != '1' {
                continue
            }
            counts++
            if j+1 < m && grid[i][j+1] == '1' && uf.union(i*m+j, i*m+j+1) {
                counts--
            }
            if i+1 < n && grid[i+1][j] == '1' && uf.union(i*m+j, (i+1)*m+j) {
                counts--
            }
        }
    }

    return counts
}

type unionFindSet struct {
    father  []int
    rank    []int
}

func newUnionFindSet(n int) *unionFindSet {
    father := make([]int, n)
    rank := make([]int, n)
    for i := range father {
        father[i] = i
        rank[i] = 1
    }
    return &unionFindSet{father, rank}
}

func (uf *unionFindSet) find(x int) int {
    if uf.father[x] != x {
        uf.father[x] = uf.find(uf.father[x])
    }
    return uf.father[x]
}

func (uf *unionFindSet) union (x, y int) bool {
    xRoot, yRoot := uf.find(x), uf.find(y)
    if xRoot == yRoot {
        return false
    }

    if uf.rank[xRoot] < uf.rank[yRoot] {
        xRoot, yRoot = yRoot, xRoot
    }

    uf.father[yRoot] = xRoot
    uf.rank[xRoot] += uf.rank[yRoot]

    return true
}
```