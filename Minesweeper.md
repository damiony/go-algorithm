扫雷游戏

----

### 题目描述

给定一个代表游戏板的二维字符矩阵。 'M' 代表一个未挖出的地雷，'E' 代表一个未挖出的空方块，'B' 代表没有相邻（上，下，左，右，和所有4个对角线）地雷的已挖出的空白方块，数字（'1' 到 '8'）表示有多少地雷与这块已挖出的方块相邻，'X' 则表示一个已挖出的地雷。

现在给出某个未挖出方块的位置，根据以下规则，返回相应位置被点击后对应的面板：

- 如果一个地雷（'M'）被挖出，游戏就结束了，把它改为 'X'。
- 如果一个没有相邻地雷的空方块（'E'）被挖出，修改它为（'B'），并且所有和其相邻的未挖出方块都应该被递归地揭露。
- 如果一个至少与一个地雷相邻的空方块（'E'）被挖出，修改它为数字（'1'到'8'），表示相邻地雷的数量。
- 如果在此次点击中，若无更多方块可被揭露，则返回面板。

示例：

```bash
输入: 
[
	['E', 'E', 'E', 'E', 'E'],
	['E', 'E', 'M', 'E', 'E'],
	['E', 'E', 'E', 'E', 'E'],
	['E', 'E', 'E', 'E', 'E']
]

Click : [3,0]

输出: 
[
	['B', '1', 'E', '1', 'B'],
	['B', '1', 'M', '1', 'B'],
	['B', '1', '1', '1', 'B'],
	['B', 'B', 'B', 'B', 'B']
]
```

----

### 解法

解法一：DFS

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
var offset = [][]int{
	{0, 1}, {0, -1}, {-1, 0}, {1, 0}, {1, 1}, {1, -1}, {-1, -1}, {-1, 1},
}

func updateBoard(board [][]byte, click []int) [][]byte {
	if board[click[0]][click[1]] == 'M' {
		board[click[0]][click[1]] = 'X'
		return board
	}
	updateBoardDfs(board, click[0], click[1])
	return board
}

func updateBoardDfs(board [][]byte, x, y int) {
	if board[x][y] != 'E' {
		return
	}
	count := 0
	for _, v := range offset {
		tmpX, tmpY := x+v[0], y+v[1]
		if tmpX < 0 || tmpX >= len(board) || tmpY < 0 || tmpY >= len(board[0]) {
			continue
		}
		if board[tmpX][tmpY] == 'M' {
			count++
		}
	}
	if count > 0 {
		board[x][y] = byte(count + '0')
		return
	}
	board[x][y] = 'B'
	for _, v := range offset {
		tmpX, tmpY := x+v[0], y+v[1]
		if tmpX < 0 || tmpX >= len(board) || tmpY < 0 || tmpY >= len(board[0]) {
			continue
		}
		if board[tmpX][tmpY] == 'E' {
			updateBoardDfs(board, tmpX, tmpY)
		}
	}
	return
}
```



### 解法二：

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(mn)`

```go
func updateBoard(board [][]byte, click []int) [][]byte {
	if board[click[0]][click[1]] == 'M' {
		board[click[0]][click[1]] = 'X'
		return board
	}

	visited := make([][]bool, len(board))
	for i := range visited {
		visited[i] = make([]bool, len(board[i]))
	}

	directionR := []int{1, -1, 0, 0, 1, 1, -1, -1}
	directionC := []int{0, 0, 1, -1, 1, -1, 1, -1}
	queue := [][]int{click}
	visited[click[0]][click[1]] = true
	for len(queue) > 0 {
		click := queue[0]
		queue = queue[1:]
		count, r, c := 0, click[0], click[1]
		for i := 0; i < 8; i++ {
			tmpR := r + directionR[i]
			tmpC := c + directionC[i]
			if tmpR < 0 || tmpR >= len(board) || tmpC < 0 || tmpC >= len(board[0]) {
				continue
			}
			if board[tmpR][tmpC] == 'M' {
				count++
			}
		}

		if count > 0 {
			board[r][c] = byte(count + '0')
		} else {
			board[r][c] = 'B'
			for i := 0; i < 8; i++ {
				tmpR := r + directionR[i]
				tmpC := c + directionC[i]
				if tmpR < 0 || tmpR >= len(board) || tmpC < 0 || tmpC >= len(board[0]) {
					continue
				}
				if board[tmpR][tmpC] == 'E' && !visited[tmpR][tmpC] {
					queue = append(queue, []int{tmpR, tmpC})
					visited[tmpR][tmpC] = true
				}
			}
		}
	}
	return board
}
```

