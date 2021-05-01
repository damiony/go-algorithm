### 题目描述

**多数元素：**

给定一个大小为`n`的数组，找到其中的多数元素，多数元素是指数组中出现次数大于`n/2`的元素。

假定数组非空，并且总是存在多数元素。

**示例：**

```shell
输入：[3, 2, 3]
输出：3
```

### 解法

**解法一：排序法：**

- 时间复杂度：`O(nlogn)`，依赖具体的排序算法，通常使用的是`O(nlogn)`复杂度的算法。
- 空间复杂度：依赖具体的排序算法。

```go
func majorityElement(nums []int) int {
	sort.Ints(nums)
	return nums[len(nums)/2]
}
```

**解法二：随机法**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func majorityElement(nums []int) int {
	rand.Seed(int64(time.Now().Unix()))
	for {
		candicate := rand.Intn(len(nums))

		total := 0
		for _, v := range nums {
			if v == nums[candicate] {
				total++
			}
			if total > len(nums)/2 {
				return v
			}
		}
	}
}
```

**解法三：哈希法**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func majorityElement(nums []int) int {
	cMap := map[int]int{}
	max := 0
	index := 0
	for i, v := range nums {
		cMap[v]++
		if cMap[v] > max {
			max = cMap[v]
			index = i
		}
	}
	return nums[index]
}
```

**解法四：投票法**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func majorityElement(nums []int) int {
	candicate := 0
	total := 0
	for _, e := range nums {
		if total == 0 {
			candicate = e
			total++
			continue
		}
		if e == candicate {
			total++
		} else {
			total--
		}
	}
	return candicate
}
```

**解法五：位运算**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func majorityElement(nums []int) int {
	result := 0
	for i := 0; i < 64; i++ {
		one := 0
		for _, ele := range nums {
			if (ele>>i)&1 == 1 {
				one++
			}
		}
		if one > len(nums)/2 {
			result += (1 << i)
		}
	}
	return result
}
```

**解法六：分治法**

- 时间复杂度：`O(nlogn)`
- 空间复杂度：`O(logn)`

```go
func majorityElement(nums []int) int {
	if len(nums) == 1 {
		return nums[0]
	}

	leftMajor := majorityElement(nums[0 : len(nums)/2])
	rightMajor := majorityElement(nums[len(nums)/2:])

	if leftMajor == rightMajor {
		return leftMajor
	}

	var leftCnt, rightCnt int
	for _, ele := range nums {
		if ele == leftMajor {
			leftCnt++
		}
		if ele == rightMajor {
			rightCnt++
		}
	}

	if leftCnt > rightCnt {
		return leftMajor
	}
	return rightMajor
}
```
