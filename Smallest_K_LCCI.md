### 题目描述

**最小`K`个数：**

找出数组中最小的`K`个数，以任意顺序返回。

**示例：**

```shell
输入：arr = [1, 3, 5, 7, 2, 4, 6, 8], k = 4
输出：[1, 2, 3, 4]
```

### 解法

**解法一：快排选择思想**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func smallestK(arr []int, k int) []int {
	n := len(arr)
	if n <= k {
		return arr
	}
	slow, fast := 0, 0
	for fast < n-1 {
		if arr[fast] <= arr[n-1] {
			arr[slow], arr[fast] = arr[fast], arr[slow]
			slow++
		}
		fast++
	}
	arr[slow], arr[n-1] = arr[n-1], arr[slow]
	if slow == k || slow+1 == k {
		return arr[:k]
	}
	if slow > k {
		return smallestK(arr[:slow], k)
	}
	ans := append(arr[:slow+1], smallestK(arr[slow+1:], k-slow-1)...)
	return ans
}
```

**解法二：排序选前K个数**

代码省略

**解法三：堆排序**

维护包含`k`个元素的大顶堆，然后返回即可。代码省略。