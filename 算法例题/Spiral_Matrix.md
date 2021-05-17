### 题目描述

**螺旋矩阵：**

给定一个`mxn`列的矩阵，按顺时针螺旋顺序，返回矩阵的所有元素。

**示例：**

```shell
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

### 解法

**解法一：按层遍历 + 数组**

使用数组记录每个点的访问历史。

**解法二：按层遍历**

- 时间复杂度：`O(mn)`
- 空间复杂度：`O(1)`

```go
func spiralOrder(matrix [][]int) []int {
	rowS, rowE := 0, len(matrix)-1
	colS, colE := 0, len(matrix[0])-1
	ans := []int{}

	for rowS <= rowE && colS <= colE {
		for i := colS; i <= colE; i++ {
			ans = append(ans, matrix[rowS][i])
		}
		for i := rowS+1; i <= rowE; i++ {
			ans = append(ans, matrix[i][colE])
		}
        if rowS < rowE {
		    for i := colE-1; i >= colS; i-- {
		    	ans = append(ans, matrix[rowE][i])
		    }
        }
        if colS < colE {
		    for i := rowE-1; i > rowS; i-- {
		    	ans = append(ans, matrix[i][colS])
		    }
        }
		colS++
		colE--
		rowS++
		rowE--
	}
	return ans
}
```