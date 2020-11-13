N 皇后

----

### 题目描述

`n`皇后问题研究的是如何将`n`个皇后放置在`nxn`的棋盘上，并且使皇后彼此之间不能相互攻击。

给定一个整数，返回所有不同的`n`皇后问题的解决方案。

每一种解法包含一个明确的`n`皇后问题的棋子放置位置，该方案中`Q`和`.`分别代表了皇后和空位。

示例：

``` bash
输入：4
输出：[
 [".Q..",  // 解法 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // 解法 2
  "Q...",
  "...Q",
  ".Q.."]
]
# 解释: 4 皇后问题存在两个不同的解法。
```



----

### 解法：

解法一：回溯法（一）

- 时间复杂度: `O(N!)`
- 空间复杂度: `O(N)`

```go
var solutions [][]string

func solveNQueens(n int) [][]string {
	solutions = [][]string{}
	column := make([]bool, n)
	diagonals1 := map[int]bool{}
	diagonals2 := map[int]bool{}
	rows := []int{}
	queens(n, rows, column, diagonals1, diagonals2)
	return solutions
}

func queens(n int, rows []int, column []bool, diagonals1, diagonals2 map[int]bool) {
	if len(rows) == n {
		tmp := genarateQueen(rows)
		solutions = append(solutions, tmp)
		return
	}
	for i := 0; i < n; i++ {
		if column[i] {
			continue
		}
		if diagonals1[i+len(rows)] || diagonals2[i-len(rows)] {
			continue
		}
		column[i] = true
		diagonals1[i+len(rows)] = true
		diagonals2[i-len(rows)] = true
		rows = append(rows, i)
		queens(n, rows, column, diagonals1, diagonals2)

		rows = rows[:len(rows)-1]
		column[i] = false
		delete(diagonals1, i+len(rows))
		delete(diagonals2, i-len(rows))
	}
	return
}

func genarateQueen(rows []int) []string {
	res := []string{}
	for _, num := range rows {
		str := ""
		for i := 0; i < len(rows); i++ {
			if num == i {
				str += "Q"
			} else {
				str += "."
			}
		}
		res = append(res, str)
	}
	return res
}
```



解法二：回溯法（二）

- 时间复杂度: `O(N!)`
- 空间复杂度: `O(N)`

```go
var solutions [][]string

func solveNQueens(n int) [][]string {
	solutions = [][]string{}
	var column, diagonals1, diagonals2 int
	rows := []int{}
	queens(n, rows, column, diagonals1, diagonals2)
	return solutions
}

func queens(n int, rows []int, column, diagonals1, diagonals2 int) {
	if len(rows) == n {
		tmp := genarateQueen(rows)
		solutions = append(solutions, tmp)
		return
	}
	availablePositions := (1<<n - 1) & (^(column | diagonals1 | diagonals2))
	for availablePositions != 0 {
		position := availablePositions & (-availablePositions)
		availablePositions = availablePositions & (availablePositions - 1)
		rows = append(rows, bits.OnesCount(uint(position-1)))
		queens(n, rows, column|position, (diagonals1|position)>>1, (diagonals2|position)<<1)

		rows = rows[:len(rows)-1]
	}
	return
}

func genarateQueen(rows []int) []string {
	res := []string{}
	for _, num := range rows {
		str := ""
		for i := 0; i < len(rows); i++ {
			if num == i {
				str += "Q"
			} else {
				str += "."
			}
		}
		res = append(res, str)
	}
	return res
}
```

