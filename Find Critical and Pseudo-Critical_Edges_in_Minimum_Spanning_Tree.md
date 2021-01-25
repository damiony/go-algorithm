### 题目描述

**找到最小生成树里的关键边和非关键边**

给定一个`n`个点的带权无向连通图，节点编号为`0`到`n-1`，同时还有一个数组`edges`，其中`edges[i] = [fromi, toi, weighti]`表示在`fromi`和`toi`节点之间有一条带权无向边。

最小生成树 (MST) 是给定图中边的一个子集，它连接了所有节点且没有环，而且这些边的权值和最小。

请找到给定图最小生成树的所有关键边和伪关键边。

如果从图中删去关键边边，会导致最小生成树的权值和增加，或者导致最小生成树不存在。伪关键边则是可能会出现在某些最小生成树中，但不会出现在所有最小生成树中。

**示例：**

```shell
输入：n = 5, edges = [[0,1,1],[1,2,1],[2,3,2],[0,3,2],[0,4,3],[3,4,3],[1,4,6]]
输出：[[0,1],[2,3,4,5]]
```

### 解法

**解法一：** 并查集 + 枚举

```go
func findCriticalAndPseudoCriticalEdges(n int, edges [][]int) [][]int {
	for i := range edges {
		edges[i] = append(edges[i], i)
	}
	sort.Slice(edges, func(i, j int) bool { return edges[i][2] < edges[j][2] })

	uf := newUnionFind(n)
	weight := generateMst(uf, edges, -1)

	keyEdges, pseudoKeyEdges := []int{}, []int{}
	for i := range edges {
		edge := edges[i][3]
		// 检查是否是关键边
		if generateMst(newUnionFind(n), edges, edge) > weight {
			keyEdges = append(keyEdges, edge)
			continue
		}
		// 检查是否是非关键边
		uf = newUnionFind(n)
		uf.union(edges[i][0], edges[i][1])
		if edges[i][2]+generateMst(uf, edges, edge) == weight {
			pseudoKeyEdges = append(pseudoKeyEdges, edge)
		}
	}
	return [][]int{keyEdges, pseudoKeyEdges}
}

type unionFind struct {
	father, rank []int
	count        int
}

func newUnionFind(n int) *unionFind {
	father, rank := make([]int, n), make([]int, n)
	for i := range father {
		father[i] = i
		rank[i] = 1
	}
	return &unionFind{father, rank, n}
}

func (u *unionFind) find(x int) int {
	if u.father[x] != x {
		u.father[x] = u.find(u.father[x])
	}
	return u.father[x]
}

func (u *unionFind) union(i, j int) bool {
	rootX, rootY := u.find(i), u.find(j)
	if rootX == rootY {
		return false
	}
	if u.rank[rootX] < u.rank[rootY] {
		rootX, rootY = rootY, rootX
	}
	u.father[rootY] = rootX
	u.rank[rootX] += u.rank[rootY]
	u.count--
	return true
}

func generateMst(uf *unionFind, edges [][]int, edge int) int {
	var weigh int
	for i := range edges {
		if edges[i][3] != edge && uf.union(edges[i][0], edges[i][1]) {
			weigh += edges[i][2]
		}
	}
	// 存在两个不连通的顶点
	if uf.count > 1 {
		return math.MaxInt64
	}
	return weigh
}
```