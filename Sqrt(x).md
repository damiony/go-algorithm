x的平方根

----

### 题目描述

计算并且返回`x`的平方根，其中`x`是非负整数，即舍去小数部分。

示例：

```bash
输入：8
输出：2
```

----

### 解法

解法一：调用库函数

虽然不能使用`sqrt`库函数，但是可以使用其他代替，不过可能会被怼。

- 时间复杂度：取决于其他库函数的实现
- 空间复杂度：取决于其他库函数的实现

```go
func mySqrt(x int) int {
	if x == 0 {
		return x
	}

	ans := int(math.Exp(0.5 * math.Log(float64(x))))
    // 浮点数精度不够，导致计算得出的ans可能偏小，所以需要考察ans+1
	if (ans+1)*(ans+1) <= x {
		ans++
	}
	return ans
}
```

方法二：二分法1

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func mySqrt(x int) int {
	left, right := 0, x
	ans := -1
	for left <= right {
		mid := left + (right-left)/2
		if mid*mid <= x {
			left = mid + 1
			ans = mid
		} else {
			right = mid - 1
		}
	}
	return ans
}
```

方法三：二分法2

这是二分法的变化写法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func mySqrt(x int) int {
	high := 0
	for (1<<high)*(1<<high) <= x {
		high++
	}

	ans := 0
	for high >= 0 {
		tmp := ans + 1<<high
		if tmp*tmp <= x {
			ans = tmp
		}
		high--
	}

	return ans
}
```

解法四：牛顿法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func mySqrt(x int) int {
	if x == 0 {
		return x
	}
	fx := float64(x)
	ans := fx
	for {
		tmp := 0.5 * (ans + fx/ans)
		if ans-tmp < 1e-7 {
			break
		}
		ans = tmp
	}
	return int(ans)
}
```

