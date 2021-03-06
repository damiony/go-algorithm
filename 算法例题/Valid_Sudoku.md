有效的数独

----

### 题目描述

判断一个 9x9 的数独是否有效。只需要根据以下规则，验证已经填入的数字是否有效即可。

- 数字 1-9 在每一行只能出现一次。

- 数字 1-9 在每一列只能出现一次。

- 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。

示例：

```bash
输入:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
输出: true
```

----

### 解法

解法一：单词遍历

- 时间复杂度：`O(1)`
- 空间复杂度：`O(1)`

```go
func isValidSudoku(board [][]byte) bool {
	rows := make([]int, 9)
	cols := make([]int, 9)
	boxs := make([]int, 9)

	for i := 0; i < 9; i++ {
		for j := 0; j < 9; j++ {
			ele := board[i][j]
			if ele == '.' {
				continue
			}
			index := 1 << (ele - '1')
			if rows[i]&index > 0 || cols[j]&index > 0 || boxs[(i/3)*3+(j/3)]&index > 0 {
				return false
			}
			rows[i] |= index
			cols[j] |= index
			boxs[(i/3)*3+(j/3)] |= index
		}
	}
	return true
}
```

