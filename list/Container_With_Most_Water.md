盛最多水的容器

----

### 题目描述

给你`n`个非负整数`a1，a2，...，an`每个数代表坐标中的一个点`(i, ai)`。在坐标内画`n`条垂直线，垂直线`i`的两个端点分别为`(i, ai)`和`(i, 0)`。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

示例：

```shell
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

----

### 解法

方法一：暴力求解(**不推荐**)

循环遍历两遍数组，求出每一个元素可能的面积组合，再比较得出最大值即可。

- 时间复杂度: O(n^2)
- 空间复杂度: O(1)



方法二：双指针法

- 时间复杂度：O(N)
- 空间复杂度：O(1)

```go
func maxArea(height []int) int {
	var max, area int

	left := 0
	right := len(height) - 1
	for left < right {
		if height[left] >= height[right] {
			area = height[right] * (right - left)
			right--
		} else {
			area = height[left] * (right - left)
			left++
		}
		if max < area {
			max = area
		}
	}

	return max
}
```