## 1. 衡量排序算法的效率

- 效率分析：

1. 最好情况、最坏情况、平均情况时间复杂度

   对于要排序的数据，有的接近有序，有的完全无序。有序度不同，对于排序的执行时间会有影响。所以需要知道排序算法在不用数据规模下的性能表现。

2. 时间复杂度的系数、常数、低阶

   实际开发中，待排序数据规模可能很小。所以进行算法分析时，就得把系数、常数、低阶考虑进来。

3. 比较次数和交换次数

   分析效率时，还应该把比较和交换次数考虑进来。

- 原地排序：

​	空间复杂度是`O(1)`的算法。

- 稳定性：

​	排序之后，相等元素之间原有的先后顺序不变，则为稳定。

## 2. 冒泡排序

- 比较相邻元素，如果不满足大小关系，就互换。

- 分析：
  - 时间复杂度为`O(1)`，是原地排序算法。
  - 相同大小的数据，在排序后不会改变顺序，是稳定的排序算法。
  - 最好情况时间复杂度`O(n)`，最坏情况时间复杂度`O(n^2)`，平均情况时间复杂度`O(n^2)`
  - 冒泡排序交换次数等于逆序度，也就是`n*(n - 1)/2 - 初始有序度`。

- 示例代码：

```go
func Bubble(nums []int) []int {
	lastEnd := len(nums) - 1
	for i := 0; i < len(nums)-1; i++ {
		exchange := false
		curEnd := 0
		for j := 0; j < lastEnd; j++ {
			if nums[j] > nums[j+1] {
				exchange = true
				curEnd = j
				nums[j], nums[j+1] = nums[j+1], nums[j]
			}
		}
		if !exchange {
			break
		}
		lastEnd = curEnd
	}
	return nums
}
```

## 3. 插入排序

将数组分为两个区间，`已排序区间`和`未排序区间`。

初始已排序区间只有一个元素，是数组第一个元素。

取未排序区间元素，在已排序区间找到合适的插入位置，插入元素，重复这个过程。

未排序区间为空，算法结束。

分析：

- 是原地排序算法
- 是稳定的排序算法
- 最好情况时间复杂度`O(n)`，最坏情况时间复杂度`O(n^2)`，平均情况时间复杂度`O(n^2)`。
- 移动的次数是固定的，等于逆序度。

示例代码：

```go
func Insertion(nums []int) []int {
	for i := 1; i < len(nums); i++ {
		j := i
		cur := nums[j]
		for j > 0 && cur < nums[j-1] {
			nums[j] = nums[j-1]
			j--
		}
		nums[j] = cur
	}
	return nums
}
```

## 4. 选择排序

从未排序区间选择最小的元素，放到已排序区间的末尾。

分析：

- 是原地排序算法
- 不稳定的排序算法
- 最好、最坏、平均情况时间复杂度`O(n^2)`

示例代码：

```go
func Selection(nums []int) []int {
	for i := 0; i < len(nums)-1; i++ {
		minIdx := i
		for j := i; j < len(nums); j++ {
			if nums[j] < nums[minIdx] {
				minIdx = j
			}
		}
		nums[i], nums[minIdx] = nums[minIdx], nums[i]
	}
	return nums
}
```

## 5. 冒泡排序 vs 插入排序

- 冒泡排序的数据交换要比插入排序的数据移动复杂，冒泡排序需要3个赋值操作，而插入排序只需要1个。

## 6. 归并排序

归并使用的是分治思想。

归并排序的地推公式为：

```shell
# 递推公式
merge_sort(p...r) = merge( merge_sort(p...q), merge_sort(q+1...r) )
# 终止条件
p >= r 不再继续分解
```

性能分析：

- 是稳定的排序算法
- 最好情况、最坏情况、平均情况时间复杂度都是`O(nlogn)`
- 空将复杂度是`O(n)`

```go
func Merge(nums []int) []int {
	n := len(nums)
	if n <= 1 {
		return nums
	}
	mid := (n - 1) >> 1
	left := append([]int(nil), nums[:mid+1]...)
	right := append([]int(nil), nums[mid+1:]...)

	left, right = Merge(left), Merge(right)
	i, j, k := 0, 0, 0
	for i < len(left) || j < len(right) {
		if i < len(left) && (j >= len(right) || left[i] < right[j]) {
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
```

## 7. 快速排序

- 思想：

  选择任意一个数据作为分区点`pivot`，遍历整个数据，将小于`pivot`的放在左边，将大于`pivot`的放在右边，将`pivot`放在中间。

  然后递归排序左边区间，和右边区间的数据，直到区间缩小为1。

- 递推公式：

  ```shell
  # 递推公式
  quick_sort(p...r) = quick_sort(p...q-1)  + quick_sort(q+1...r)
  # 终止条件
  p >= r
  ```

- 性能分析：

  - 是原地、不稳定的排序算法
  - 最坏情况下的时间复杂度为`O(n^2)`，最好情况、平均情况下的时间复杂度都是`O(nlogn)`。时间复杂度退化到`O(n^2)`的概率非常小。

- 代码示例：

```go
func Quick(nums []int) []int {
	if len(nums) <= 1 {
		return nums
	}
	p := partition(nums)
	Quick(nums[:p])
	Quick(nums[p+1:])
	return nums
}

func partition(nums []int) int {
	n := len(nums)
	pivot := nums[n-1]
	i := 0
	for j := 0; j < n-1; j++ {
		if nums[j] < pivot {
			nums[i], nums[j] = nums[j], nums[i]
			i++
		}
	}
	nums[i], nums[n-1] = nums[n-1], nums[i]
	return i
}
```

## 8. 桶排序

- 思想：

  将要排序的数据分到几个有序的桶里，每个桶的数据再进行单独排序，一般是快速排序。

  桶内排完序后，再把桶内的数据按照顺序依次取出，组成的序列就是有序的。

- 适用场景：

  待排序数据需要很容易就能划分成 m 个桶，并且，桶与桶之间有着天然的大小顺序。

  数据在各个桶之间的分布是比较均匀的。

  桶排序比较适合用在外部排序中。外部排序就是数据存储在外部磁盘中，数据量比较大，内存有限，无法将数据全部加载到内存中。

- 性能分析：

  - 时间复杂度：`O(n)`

## 9. 计数排序

- 思想：

  计数排序其实就是桶排序的一种特殊情况。当要排序的数据所处范围并不大时，比如最大值是`K`，就可以将数据划分成`K`个桶，每个桶内的数据值都是相同的。

- 适用场景：

  数据范围并不大，如果数据范围比要排序数据大很多，就不适用计数排序了。

  只能给非负整数排序，如果待排序数据是其他类型，要将其在不改变相对大小的情况下，转化为非负整数。

- 性能分析：

  - 是非原地，稳定的排序算法。
  - 时间复杂度：`O(n)`
  
- 代码实现

  ```go
  func CountSort(nums []int) []int {
  	n := len(nums)
  	if n <= 1 {
  		return nums
  	}
  	max := nums[0]
  	min := nums[0]
  	for _, cur := range nums {
  		if cur > max {
  			max = cur
  		}
  		if cur < min {
  			min = cur
  		}
  	}
  
  	count := make([]int, max-min+1)
  	for _, cur := range nums {
  		count[cur-min]++
  	}
  
  	for i := 1; i < len(count); i++ {
  		count[i] += count[i-1]
  	}
  
  	ans := make([]int, n)
  	for i := n - 1; i >= 0; i-- {
  		idx := nums[i] - min
  		count[idx]--
  		ans[count[idx]] = nums[i]
  	}
  	return ans
  }
  ```

## 9. 基数排序

- 适用场景：

  数据需要可以分割出独立的“位”来比较，并且位之间有递进的关系。

  每一位的数据范围不能太大，要可以用线性排序算法来排序，且每一位的排序算法要是稳定的。

- 性能分析：

  - 是非原地、稳定的排序算法。
  - 时间复杂度：`O(dn), d是每一位数据的数据集大小`
  - 空间复杂度：`O(d)`

### 算法总结

|          | 时间复杂度            | 是稳定排序 | 是原地排序 |
| :------: | :-------------------- | ---------- | ---------- |
| 冒泡排序 | `O(n^2)`              | yes        | yes        |
| 插入排序 | `O(n^2)`              | yes        | yes        |
| 选择排序 | `O(n^2)`              | no         | yes        |
| 快速排序 | `O(nlogn)`            | no         | yes        |
| 归并排序 | `O(nlogn)`            | yes        | no         |
| 计数排序 | `O(n+k)`，k是数据范围 | yes        | no         |
|  桶排序  | `O(n)`                | yes        | no         |
| 基数排序 | `O(dn)` d是维度       | yes        | no         |

