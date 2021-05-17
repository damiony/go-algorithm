### 题目描述

**爬楼梯：**

假设你正在爬楼梯，需要`n`级才能到达顶楼。每次可以爬`1`或者`2`个台阶，有多少种不同的方法爬到楼顶？

**示例：**

```shell
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
```

```shell
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

### 解法

**方法一：递归**

- 时间复杂度：O(2^n)
- 空间复杂度：O(n)

```go
func climbStairs(n int) int {
    if n <= 2 {
        return n
    }

    return climbStairs(n - 1) + climbStairs(n - 2)
}
```

注意：这种解法时间复杂度太高，效率很差

**方法二：递归，保存中间结果**

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func climbStairs(n int) int {
    history := make(map[int]int)
    return climbStairsInternally(n, history)
}

func climbStairsInternally(n int, history map[int]int) int{
    if v, ok := history[n]; ok {
        return v
    }
    if n <= 2 {
        history[n] = n
        return n
    }

    steps := climbStairsInternally(n - 1, history) + climbStairsInternally(n - 2, history)
    history[n] = steps
    return steps
}
```

**方法三：迭代，也可以称为动态规划**

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func climbStairs(n int) int {
    if n <= 2 {
        return n
    }

    step1 := 1
    step2 := 2

    result := 0
    for n > 2 {
        result = step1 + step2
        step1, step2 = step2, result
        n--
    }
    return result
}
```

**方法四：斐波那契数列通项公式**

该方法当`n`很大时，误差也会变大，无法通过`leetcode`的执行。

```go
func climbStairs(n int) int {
    var nFloat = float64(n)
    var xQrt = math.Sqrt(5)

    var c1 = math.Pow((1+xQrt)/2, nFloat + 1)
    var c2 = math.Pow((1-xQrt)/2, nFloat + 1)
    var result = (1 / xQrt) * (c1 - c2)

    return int(result)
}
```

**方法五：矩阵快速幂**

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func climbStairs(n int) int {
	matrix := [][]int{
		{1, 1},
		{1, 0},
	}
	ans := matrix
	temp := matrix
	for n > 0 {
		if n&1 == 1 {
			ans = multiple(ans, temp)
		}
		temp = multiple(temp, temp)
		n = n >> 1
	}
	return ans[1][0] * 1
}

func multiple(a [][]int, b [][]int) [][]int {
	temp := make([][]int, 2)
	for i := range temp {
		temp[i] = make([]int, 2)
	}
	for i := 0; i < 2; i++ {
		for j := 0; j < 2; j++ {
			temp[i][j] = a[i][0]*b[0][j] + a[i][1]*b[1][j]
		}
	}
	return temp
}
```
