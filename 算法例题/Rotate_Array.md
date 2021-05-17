### 题目描述

**旋转数组**

给定一个数组，将数组中的元素向右移动`k`个位置，其中`k`是非负数。

要求空间复杂度为`O(1)`

**示例**

```bash
输入：nums = [1, 2, 3, 4, 5, 6, 7], k = 3
输出：[5, 6, 7, 1, 2, 3, 4]
```

### 解法

**解法一**：三次旋转

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func rotate(nums []int, k int) {
	n := len(nums)
	k = k % n
	for i := 0; i <= (n-1)>>1; i++ {
		nums[i], nums[n-1-i] = nums[n-1-i], nums[i]
	}
	for i := 0; i <= (k-1)>>1; i++ {
		nums[i], nums[k-1-i] = nums[k-1-i], nums[i]
	}
	for i := 0; i <= (n-k-1)>>1; i++ {
		nums[i+k], nums[n-1-i] = nums[n-1-i], nums[i+k]
	}
	return
}
```

**解法二**：顺序替换

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(1)`

```go
func rotate(nums []int, k int) {
	n := len(nums)
	k = k % n
	window := n - k
	for i := window; i < n; i++ {
		cur := nums[i]
		for j := i; j > i-window; j-- {
			nums[j] = nums[j-1]
		}
		nums[i-window] = cur
	}
	return
}
```

**解法三**：环状替换

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func rotate(nums []int, k int) {
	n := len(nums)
	k = k % n
	if k <= 0 {
		return
	}
	for start, end := 0, gcd(n, k); start < end; start++ {
		num, next := nums[start], (start+k)%n
		for next != start {
			nums[next], num = num, nums[next]
			next = (next + k) % n
		}
		nums[next] = num
	}
	return
}

func gcd(a, b int) int {
	for a != b {
		if a > b {
			a -= b
		} else {
			b -= a
		}
	}
	return a
}
```

**解法四**：块状替换

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func rotate(nums []int, k int) {
	n := len(nums)
	k = k % n
	if k <= 0 {
		return
	}

	idx := 0
	for idx+k < n {
		w := n - k - idx
		end := idx + w
		if end > idx+k {
			end = idx + k
		}
		for start := idx; start < end; start++ {
			nums[start], nums[start+w] = nums[start+w], nums[start]
		}

		if w >= k {
			idx += k
		} else {
			idx += w
			k -= w
		}
	}
	return
}
```

