单词搜索

----

### 题目描述

给定一个二维网格和一个单词，找出该单词是否存在于网格中。单词必须由相邻单元格的字母构成。

示例：

```bash
输入：
board = [
  ['A', 'B', 'C', 'E'],
  ['S', 'F', 'C', 'S'],
  ['A', 'D', 'E', 'E']
]
word = "ABCCED"
输出：true
```

----

### 解法

解法一：回溯

- 时间复杂度：`O(nm * 3^L)`，n和m表示网格长、宽，L表示单词长度。
- 空间复杂度：`O(nm)`

```go
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

