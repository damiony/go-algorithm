组合

----

### 题目描述

给定两个整数`n`和`k`，返回`1...n`中所有可能的`k`个数组合。

示例：

```shell
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

----

### 解法

解法一：回溯

```go
func combine(n int, k int) [][]int {
	result := [][]int{}
	if k <= 0 || n < k {
		return result
	}

	maybe := []int{}
	return helper(n, k, 1, maybe, result)
}

func helper(n int, k int, start int, maybe []int, result [][]int) [][]int {
	if len(maybe)+(n-start+1) < k {
		return result
	}
	if len(maybe) == k {
		c := append([]int{}, maybe...)
		result = append(result, c)
		return result
	}
	for i := start; i <= n; i++ {
		maybe = append(maybe, i)
		result = helper(n, k, i+1, maybe, result)

		maybe = maybe[:len(maybe)-1]
	}
	return result
}
```



解法二：递归（1）

```go
func combine(n int, k int) [][]int {
	result := [][]int{}
	if k <= 0 || n < k {
		return result
	}

	maybe := []int{}
	return helper(n, k, 1, maybe, result)
}

func helper(n int, k int, start int, maybe []int, result [][]int) [][]int {
	if len(maybe)+(n-start+1) < k {
		return result
	}
	if len(maybe) == k {
		c := append([]int{}, maybe...)
		result = append(result, c)
		return result
	}
	maybe = append(maybe, start)
	result = helper(n, k, start+1, maybe, result)

	maybe = maybe[:len(maybe)-1]
	result = helper(n, k, start+1, maybe, result)

	return result
}
```



解法五：递归（2）

```go
func combine(n int, k int) [][]int {
	result := [][]int{}
	if k == n || k == 0 {
		maybe := []int{}
		for i := 1; i <= k; i++ {
			maybe = append(maybe, i)
		}
		result = append(result, maybe)
		return result
	}

	result = combine(n-1, k-1)
	for i := range result {
		result[i] = append(result[i], n)
	}

	result = append(result, combine(n-1, k)...)
	return result
}
```



解法四：迭代（1）

```go
func combine(n int, k int) [][]int {
	result := [][]int{}
	if k <= 0 || n < k {
		return result
	}
	index := 0
	c := make([]int, k)
	for index >= 0 {
		if n-c[index]+index+1 < k {
			index--
			continue
		}
		c[index]++
		if c[index] > n {
			index--
		} else if index == k-1 {
			tmp := append([]int{}, c...)
			result = append(result, tmp)
		} else {
			index++
			c[index] = c[index-1]
		}
	}

	return result
}
```



解法五：迭代（2）

```go
func combine(n int, k int) [][]int {

	result := [][]int{}
	if k <= 0 || n < k {
		return result
	}
	for i := 1; i <= n-k+1; i++ {
		tmp := []int{i}
		result = append(result, tmp)
	}
	for i := 2; i <= k; i++ {
		for _, v := range result {
			endV := v[len(v)-1]
			endI := n - (k - i)
			for j := endV + 1; j <= endI; j++ {
				tmp := append([]int{}, v...)
				tmp = append(tmp, j)
				result = append(result, tmp)
			}
			result = result[1:]
		}
	}
	return result
}
```
