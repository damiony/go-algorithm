### 题目描述

**除法求值**

用一个变量对数组`equations`和一个实数值数组`values`作为已知条件，其中`equations[i] = [Ai, Bi]`和`values[i]`共同表示等式`Ai / Bi = values[i]`。每个`Ai`或`Bi`是一个表示单个变量的字符串。

用一个数组`queries`表示问题，其中`queries[j] = [Cj, Dj]`表示第 j 个问题，请你根据已知条件求`Cj / Dj = ?`，结果作为数组返回。

如果存在某个无法确定的答案，则用`-1.0`替代。假定不会出现除数为 0 的情况，且不存在任何矛盾的结果。

**示例**：

```bash
输入：equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
输出：[6.00000,0.50000,-1.00000,1.00000,-1.00000]
解释：
条件：a / b = 2.0, b / c = 3.0
问题：a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
结果：[6.0, 0.5, -1.0, 1.0, -1.0 ]
```

### 解法

**解法一：**图

需要构造成图，并且用`bfs`进行搜索，代码较麻烦不想写。

**解法二：**数组

- 时间复杂度：`O()`，待讨论
- 空间复杂度：`O()`，待讨论

```go
func calcEquation(equations [][]string, values []float64, queries [][]string) []float64 {
	toId := map[string]int{}
	for _, v := range equations {
		for _, e := range v {
			if toId[e] == 0 {
				toId[e] = len(toId) + 1
			}
		}
	}

	n := len(toId)
	graph := make([][]float64, n)
	for i := range graph {
		graph[i] = make([]float64, n)
		graph[i][i] = 1
	}
	for i, v := range equations {
		from, to := toId[v[0]], toId[v[1]]
		graph[from-1][to-1] = values[i]
		graph[to-1][from-1] = 1 / values[i]
	}
	// floyd算法
	for k := 0; k < n; k++ {
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				if graph[i][k] > 0 && graph[k][j] > 0 {
					graph[i][j] = graph[i][k] * graph[k][j]
				}
			}
		}
	}

	ans := make([]float64, len(queries))
	for i, v := range queries {
		from, to := toId[v[0]], toId[v[1]]
		if from > 0 && to > 0 && graph[from-1][to-1] > 0 {
			ans[i] = graph[from-1][to-1]
		} else {
			ans[i] = -1.0
		}
	}
	return ans
}
```

**解法三：**带权并查集

- 时间复杂度：`O()`，待讨论
- 空间复杂度：`O()`，待讨论

```go
func calcEquation(equations [][]string, values []float64, queries [][]string) []float64 {
    // 将字符转换成图节点的ID
	toId := map[string]int{}
	for _, v := range equations {
		for _, e := range v {
			if toId[e] == 0 {
				toId[e] = len(toId) + 1
			}
		}
	}
	// 初始化并查集
	set := create(len(toId) + 1)
	for i, v := range equations {
		from, to, val := toId[v[0]], toId[v[1]], values[i]
		set.add(from)
		set.add(to)
		set.union(from, to, val)
	}
	// 计算结果
	ans := make([]float64, len(queries))
	for i, v := range queries {
		from, to := toId[v[0]], toId[v[1]]
		if from > 0 && to > 0 {
			ans[i] = set.cal(from, to)
		} else {
			ans[i] = -1.0
		}
	}
	return ans
}

type unionFindSet struct {
	father []int
	weight []float64
}

func create(n int) *unionFindSet {
	return &unionFindSet{
		father: make([]int, n),
		weight: make([]float64, n),
	}
}

func (u *unionFindSet) add(id int) {
	if u.father[id] > 0 {
		return
	}
	u.father[id] = id
	u.weight[id] = 1
	return
}

// 查找成员 同时进行压缩
func (u *unionFindSet) find(x int) int {
	if u.father[x] != x {
		fa := u.find(u.father[x])
		u.weight[x] *= u.weight[u.father[x]]
		u.father[x] = fa
	}
	return u.father[x]
}

// 将不同集合进行合并 并且更新祖父节点的权重
func (u *unionFindSet) union(x, y int, val float64) {
	xRoot := u.find(x)
	yRoot := u.find(y)
	if xRoot == yRoot {
		return
	}
	u.father[xRoot] = yRoot
	u.weight[xRoot] = val * u.weight[y] / u.weight[x]
	return
}

// 计算权重的比值
func (u *unionFindSet) cal(x, y int) float64 {
	if u.find(x) != u.find(y) {
		return -1.0
	}
	return u.weight[x] / u.weight[y]
}
```

