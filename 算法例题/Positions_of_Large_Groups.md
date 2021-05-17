### 题目描述

**较大分组的位置**

在一个由小写字母构成的字符串`s`中，包含由一些连续相同字符构成的分组。分组由区间`[start, end]`表示，其中`start`和`end`分别表示该分组的起始和终止位置下标。

找到每一个较大分组的区间，并按起始下标递增排序。较大分组是指包含大于或等于三个连续字符的分组。

**示例**

```bash
输入：s = "abbxxxxzzy"
输出：[[3, 6]]
```

### 解法

**解法一**：双指针遍历

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func largeGroupPositions(s string) [][]int {
	var ans [][]int
	n, fast, slow := len(s), 0, 0
	for fast < n {
		if s[fast] != s[slow] {
			slow = fast
			continue
		}

		fast++
		if (fast >= n || s[fast] != s[slow]) && fast-slow >= 3 {
			ans = append(ans, []int{slow, fast - 1})
		}
	}
	return ans
}
```

