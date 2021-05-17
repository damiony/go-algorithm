### 题目描述

**斐波拉契数**

用`F(n)`表示斐波拉契数形成的序列，该数列从`0`和`1`开始，后面的每一项是前面两项数字的和。

递推式为:

```bash
F(0) = 0, F(1) = 1
F(n) = F(n-1) + F(n-2), 其中n>1
```

**示例**：

```bash
输入：2
输出：1
```

### 解法

**解法一**：暴力递归

- 时间复杂度：`O(2^n)`
- 空间复杂度：`O(n)`

```go
func fib(n int) int {
    if n <= 1 {
        return n
    }
	return dfs(n-1) + dfs(n-2)
}

func dfs(n int) int {
	if n <= 0 {
		return 0
	}
    if n == 1 {
        return 1
    }
	return dfs(n-1) + dfs(n-2)
}
```

**解法二**：递归 + 备忘录

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func fib(n int) int {
	if n <= 1 {
		return n
	}
	rec := make(map[int]int)
	return dfs(n-1, rec) + dfs(n-2, rec)
}

func dfs(n int, rec map[int]int) int {
	if n <= 0 {
		return 0
	}
	if n == 1 {
		return 1
	}
	if rec[n] > 0 {
		return rec[n]
	}

	ans := dfs(n-1, rec) + dfs(n-2, rec)
	rec[n] = ans
	return rec[n]
}
```

**解法三**：迭代（利用了动态规划的思想）

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func fib(n int) int {
	if n <= 1 {
		return n
	}

	first, second := 0, 1
	for i := 2; i <= n; i++ {
		second, first = first+second, second
	}
	return second
}
```

**解法四**：矩阵快速幂

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go

func fib(n int) int {
	if n <= 1 {
		return n
	}
	matrix := [2][2]int{
		{1, 1},
		{1, 0},
	}

	ans := [2][2]int{
		{1, 0},
		{0, 1},
	}
	i := n - 1
	for i > 0 {
		if i&1 == 1 {
			ans = matrixMulti(ans, matrix)
		}
		matrix = matrixMulti(matrix, matrix)
		i >>= 1
	}
	return ans[0][0]
}

func matrixMulti(left, right [2][2]int) [2][2]int {
	ans := [2][2]int{}
	for i := 0; i < 2; i++ {
		for j := 0; j < 2; j++ {
			ans[i][j] = left[i][0]*right[0][j] + left[i][1]*right[1][j]
		}
	}
	return ans
}
```

**解法五**：通项公式

- 时间复杂度：与`cpu`指令集有关，不讨论
- 空间复杂度：`O(1)`

```go
func fib(n int) int {
	if n <= 1 {
		return n
	}
	sqrt5 := math.Sqrt(5)
	a1 := math.Pow((1+sqrt5)/2, float64(n))
	a2 := math.Pow((1-sqrt5)/2, float64(n))
	ans := (a1 - a2) / sqrt5
	return int(math.Round(ans))
}
```

