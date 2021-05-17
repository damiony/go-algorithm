### 题目描述

**合并区间：**

给出一组区间，请合并所有重叠的区间。

**示例：**

```shell
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
```

### 解法

**解法一：排序 + 遍历**

- 时间复杂度：`O(nlogn)`
- 空间复杂度：`O(1)`

```go
func merge(intervals [][]int) [][]int {
	sort.Slice(intervals, func(i, j int) bool { return intervals[i][0] < intervals[j][0] })

	ans := [][]int{ intervals[0] }
	for i := 1; i < len(intervals); i++ {
		pre := ans[len(ans)-1]
		if intervals[i][0] > pre[1] { // 如果与前一个区间无重叠部分
			ans = append(ans, intervals[i])
		} else if intervals[i][1] > pre[1] { // 如果大于前一个区间的结尾元素
			pre[1] = intervals[i][1]
		}
	}

	return ans
}
```