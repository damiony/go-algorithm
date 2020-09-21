Pow(x, n)

----

### 题目描述

实现一个函数，用于计算x的n次幂。

示例1：

```shell
输入：2.00000, 10
输出：1024.00000
```

示例2：

```shell
输入：2.10000, 3
输出：9.26100
```

示例3：

```shell
输入：2.00000, -2
输出：0.25000
```

----

### 解法

解法一：快速幂乘 + 递归

* 时间复杂度：`O(logn)`
* 空间复杂度：`O(logn)`

```go
func myPow(x float64, n int) float64 {
	if n == 0 {
		return 1
	}
	if n < 0 {
		x = 1 / x
		n = -n
	}
	if n&1 == 1 {
		return x * myPow(x*x, n/2)
	}
	return myPow(x*x, n/2)
}
```



解法二：快速幂乘 + 迭代

* 时间复杂度：`O(logn)`
* 空间复杂度：`O(1)`

```go
func myPow(x float64, n int) float64 {
	if n < 0 {
		x = 1 / x
		n = -n
	}

	var result float64 = 1
	var xContribution float64 = x
	for n > 0 {
		if n&1 == 1 {
			result *= xContribution
		}
		xContribution *= xContribution
		n >>= 1
	}
	return result
}
```

