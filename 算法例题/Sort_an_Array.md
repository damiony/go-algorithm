### 题目描述

**排序数组：**

给定一个整数数组`nums`，将数组升序排列。

**示例：**

```shell
输入：nums = [5, 2, 3, 1]
输出：[1, 2, 3, 5]
```

### 解法

**解法：**

冒泡排序、插入排序、选择排序、归并排序、快速排序

```go
func sortArray(nums []int) []int {
	// bubbleSort(nums)
	// insertSort(nums)
	// selectSort(nums)
	// mergeSort(nums)
	quickSort(nums)
	return nums
}

func bubbleSort(nums []int) {
	n := len(nums)
	lastExchange := n
	for i := 0; i < n-1; i++ {
		fin := true
		curExchange := 0
		for j := 0; j < lastExchange-1 && j < n-1-i; j++ {
			if nums[j] > nums[j+1] {
				nums[j], nums[j+1] = nums[j+1], nums[j]
				fin = false
				curExchange = j + 1
			}
		}
		if fin {
			break
		}
		lastExchange = curExchange
	}
	return
}

func insertSort(nums []int) {
	n := len(nums)
	for i := 1; i < n; i++ {
		idx := i
		cur := nums[i]
		for idx > 0 && nums[idx-1] > cur {
			nums[idx] = nums[idx-1]
			idx--
		}
		nums[idx] = cur
	}
	return
}

func selectSort(nums []int) {
	n := len(nums)
	for i := 0; i < n; i++ {
		min := i
		for j := i; j < n; j++ {
			if nums[min] > nums[j] {
				min = j
			}
		}
		nums[min], nums[i] = nums[i], nums[min]
	}
	return
}

func mergeSort(nums []int) {
	if len(nums) <= 1 {
		return
	}

	mid := len(nums) >> 1
	left := append([]int{}, nums[:mid]...)
	right := append([]int{}, nums[mid:]...)
	mergeSort(left)
	mergeSort(right)

	ldx, rdx, idx := 0, 0, 0
	for ldx < len(left) || rdx < len(right) {
		if ldx >= len(left) || (rdx < len(right) && right[rdx] < left[ldx]) {
			nums[idx] = right[rdx]
			rdx++
		} else {
			nums[idx] = left[ldx]
			ldx++
		}
		idx++
	}
	return
}

func quickSort(nums []int) {
	n := len(nums)
	if n <= 1 {
		return
	}

	pivot := nums[n-1]
	slow := 0
	for fast := 0; fast < n-1; fast++ {
		if nums[fast] < pivot {
			nums[slow], nums[fast] = nums[fast], nums[slow]
			slow++
		}
	}
	nums[slow], nums[n-1] = nums[n-1], nums[slow]

	quickSort(nums[:slow])
	quickSort(nums[slow+1:])

	return
}
```