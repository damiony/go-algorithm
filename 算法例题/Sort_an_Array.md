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

冒泡排序，选择排序，插入排序，希尔排序，快速排序，归并排序，堆排序

```go
func sortArray(nums []int) []int {
    // return bubbleSort(nums) // 冒泡排序
    // return selectSort(nums) // 选择排序
    // return insertSort(nums) // 插入排序
    // return shellSort(nums) // 希尔排序
    // return quickSort(nums) // 快速排序
    // return mergeSort(nums) // 归并排序
    return heapSort(nums) // 堆排序
}

// 冒泡排序
func bubbleSort(nums []int) []int {
    end := len(nums)-1
    flag := false
    for i := 1; i < len(nums); i++ {
        oldEnd := end
        for j := 0; j < oldEnd; j++ {
            if nums[j] > nums[j+1] {
                nums[j], nums[j+1] = nums[j+1], nums[j]
                end = j+1
                flag = true
            }
        }
        if !flag {
            break
        }
    }
    return nums
}

// 选择排序
func selectSort(nums []int) []int {
    for i := 0; i < len(nums); i++ {
        min := i
        for j := min; j < len(nums); j++ {
            if nums[j] < nums[min] {
                min = j
            }
        }
        if min != i {
            nums[i], nums[min] = nums[min], nums[i]
        }
    }
    return nums
}

// 插入排序
func insertSort(nums []int) []int {
    for i := 1; i < len(nums); i++ {
        tmp := nums[i]
        j := i-1
        for j >= 0 && nums[j] > tmp {
            nums[j+1] = nums[j]
            j--
        }
        nums[j+1] = tmp
    }
    return nums
}

// 希尔排序
func shellSort(nums []int) []int {
    n := len(nums)
    for gap := n/2; gap >= 1; gap /= 2 {
        for i := gap; i < n; i++ {
            tmp := nums[i]
            j := i-gap
            for j >= 0 && nums[j] > tmp {
                nums[j+gap] = nums[j]
                j -= gap
            }
            nums[j+gap] = tmp
        }
    }
    return nums
}

// 快速排序
func quickSort(nums []int) []int {
    return quick(nums, 0, len(nums)-1)
}

func quick(nums []int, start, end int) []int {
    if start >= end {
        return nums
    }
    pivot := end
    i, j := start, start
    for ; j < pivot; j++ {
        if nums[j] < nums[pivot] {
            nums[i], nums[j] = nums[j], nums[i]
            i++
        }
    }
    nums[i], nums[pivot] = nums[pivot], nums[i]
    quick(nums, start, i-1)
    quick(nums, i+1, end)
    return nums
}

// 归并排序
func mergeSort(nums []int) []int {
    n := len(nums)
    if n <= 1 {
        return nums
    }

    left := append([]int{}, nums[:n/2]...)
    right := append([]int{}, nums[n/2:]...)
    mergeSort(left)
    mergeSort(right)

    i, j, k := 0, 0, 0
    for i < len(left) || j < len(right) {
        if j >= len(right) || (i < len(left) && left[i] < right[j]) {
            nums[k] = left[i]
            i++
        } else {
            nums[k] = right[j]
            j++
        }
        k++
    }
    return nums
}

// 堆排序
func heapSort(nums []int) []int {
    n := len(nums)
    // 堆化
    for i := (n-1)/2; i >= 0; i-- {
        heapifyDown(nums, i, n)
    }
    // 排序
    for i := n-1; i > 0; i-- {
        nums[i], nums[0] = nums[0], nums[i]
        heapifyDown(nums, 0, i)
    }
    return nums
}

func heapifyDown(nums []int, start int, end int) []int {
    n := end
    i := start
    for {
        max := 2*i+1
        if max >= n || max < 0 {
            break
        }
        if 2*i+2 < n && nums[2*i+2] > nums[max] {
            max = 2*i+2
        }
        if nums[max] <= nums[i] {
            break
        }
        nums[i], nums[max] = nums[max], nums[i]
        i = max
    }
    return nums
}
```