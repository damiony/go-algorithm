有效的完全平方数

----

### 题目描述

给定一个正整数，判断是否为完全平方数。不要使用库函数。

示例：

```bash
输入：15
输出：false
```

----

### 解法

解法一：牛顿法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func isPerfectSquare(num int) bool {
    if num < 2 {
        return true
    }
    x := num / 2
	for x*x > num {
		x = (x + num/x) / 2
	}
	return x*x == num
}
```

解法二：二分法

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func isPerfectSquare(num int) bool {
	left, right := 0, num
	for left <= right {
		mid := left + (right-left)/2
		if mid*mid == num {
			return true
		}
		if mid*mid > num {
			right = mid - 1
		} else {
			left = mid + 1
		}
	}
	return false
}
```

解法三：二分法2

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func isPerfectSquare(num int) bool {
	ans := 0
	for i := 30; i >= 0; i-- {
		tmp := ans + (1 << i)
		if tmp*tmp > num {
			continue
		}
		if tmp*tmp == num {
			return true
		}
        ans = tmp
	}
	return false
}
```

解法四：利用`1 + 3 + ... + (2n-1) = n^2`

- 时间复杂度：`O(logn)`
- 空间复杂度：`O(1)`

```go
func isPerfectSquare(num int) bool {
	layer := 1
	for num > 0 {
		num -= 2*layer - 1
		layer++
	}
	return num == 0
}
```

