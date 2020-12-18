单词搜索2

----

### 题目描述

给定一个 `m x n` 二维字符网格 `board` 和一个单词（字符串）列表 `words`，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

示例：

```bash
输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
输出：["eat","oath"]
```

----

### 解法

解法一：回溯

- 时间复杂度：累了，分析不动了，待定。
- 空间复杂度：累了，分析不动了，待定。

```go
func findWords(board [][]byte, words []string) []string {
	ans := []string{}
	for _, w := range words {
		if exist(board, w) {
			ans = append(ans, w)
		}
	}
	return ans
}

func exist(board [][]byte, word string) bool {
    n, m := len(board), len(board[0])
	vis := make([][]bool, n)
    for i := range vis {
        vis[i] = make([]bool, m)
    }

	dire := [][]int{
		{0, 1},  // 右
		{0, -1}, // 左
		{1, 0},  // 下
		{-1, 0}, // 上
	}
	var dfs func(row, col, idx int) bool
	dfs = func(row, col, idx int) bool {
		if word[idx] != board[row][col] {
			return false
		}
		if idx == len(word)-1 {
			return true
		}
		vis[row][col] = true
		for _, e := range dire {
			nrow := row + e[0]
			ncol := col + e[1]
			if nrow < 0 || nrow >= n || ncol < 0 || ncol >= m || vis[nrow][ncol] {
				continue
			}
			if dfs(nrow, ncol, idx+1) {
				return true
			}
		}
		vis[row][col] = false
		return false
	}
	for r := range board {
		for c := range board[r] {
			if dfs(r, c, 0) {
				return true
			}
		}
	}
	return false
}
```

