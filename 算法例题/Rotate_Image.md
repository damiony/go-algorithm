### 题目描述

**顺时针旋转矩阵**

给定一个`n × n`的二维矩阵`matrix`。请将矩阵顺时针旋转`90`度。

**示例：**

```shell
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

### 解法

**解法一：使用额外数组**

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(n^2)`

```go
func rotate(matrix [][]int) {
	n := len(matrix)
	brother := make([][]int, n)
	for i := range brother {
		brother[i] = make([]int, n)
	}

	for i := range matrix {
		for j := range matrix[i] {
			brother[j][n-1-i] = matrix[i][j]
		}
	}

	for i := range brother {
		copy(matrix[i], brother[i])
	}

	return
}
```

**解法二：利用旋转规律**

`matrix[row][col]`旋转后的位置为`matrix[col][n-1-row]`，利用此性质进行原地旋转。

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(1)`

```go
func rotate(matrix [][]int) {
	n := len(matrix)
	for i := 0; i < n/2; i++ {
		for j := 0; j < (n+1)/2; j++ {
			tmp := matrix[n-1-j][i]
			// 旋转
			matrix[n-1-j][i] = matrix[n-1-i][n-1-j]
			matrix[n-1-i][n-1-j] = matrix[j][n-1-i]
			matrix[j][n-1-i] = matrix[i][j]
			matrix[i][j] = tmp
		}
	}
	return
}
```

**解法三：利用反转规律**

先上下反转，再对角线反转，本质上还是利用了解法二的规律

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(1)`

```go
func rotate(matrix [][]int) {
	n := len(matrix)
	// 上下反转
	for i := 0; i < n>>1; i++ {
		matrix[i], matrix[n-1-i] = matrix[n-1-i], matrix[i]
	}
	// 对角线反转
	for i := 0; i < n; i++ {
		for j := 0; j < i; j++ {
			matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
		}
	}

	return
}
```