### 题目描述

**最长递增子序列：**

给定数组arr，设长度为n，输出arr的最长递增子序列。（如果有多个答案，请输出其中字典序最小的）

**示例：**

```shell
输入：[2, 1, 5, 3, 6, 4, 8, 9, 7]
输出：[1, 3, 4, 8, 9]
```

### 解法

**解法一：贪心 + 二分法**

```go
func LIS(arr []int) []int {
	// write code here
	n := len(arr)
	if n == 0 {
		return nil
	}

	max := make([]int, n)
	sub := []int{arr[0]}
	max[0] = 1
	for i := 1; i < n; i++ { 
		if arr[i] > sub[len(sub)-1] {
			sub = append(sub, arr[i])
			max[i] = len(sub)
		} else {
			l, r := 0, len(sub)
			for l < r {
				m := l + (r-l)>>1
				if sub[m] >= arr[i] {
					r = m
				} else {
					l = m + 1
				}
			}
			sub[l] = arr[i]
			max[i] = l + 1
		}
	}

	for i, j := len(max)-1, len(sub); j > 0; i-- {
		if max[i] == j {
			j--
			sub[j] = arr[i]
		}
	}
	return sub
}
```