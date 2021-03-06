### 题目描述

**只出现一次的数字：**

给定一个非负整数数组，除了某个元素只出现一次，其余元素均出现两次。

找出只出现一次的元素。

**示例：**

```shell
输入：[2, 2, 1]
输出：1
```

### 解法

如果不考虑时间复杂度和空间复杂度，解法很多。

但是如果限制时间复杂度为`O(n)`，空间复杂度为`O(1)`，则使用异或。

**解法一：**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func singleNumber(nums []int) int {
	ans := 0
	for _, v := range nums {
		ans ^= v
	}
	return ans
}
```