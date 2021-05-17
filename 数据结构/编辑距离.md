## 什么是编辑距离

编辑距离是指，将一个字符串转化成另一个字符串，需要的最少编辑次数。编辑包括增加一个字符、删除一个字符、替换一个字符。

编辑距离越大，说明两个字符串的相似程度越小；反之，编辑距离越小，说明两个字符串的相似程度越大。如果两个字符串完全相同，编辑距离就是0。

## 编辑距离的计算方式

- 莱文斯坦距离

  允许增加、删除、替换字符这三个编辑操作。

  莱文斯坦距离的大小，表示两个字符串差异的大小。

  比如：`mitcmu`和`mtacnu`的莱文斯坦距离是3。

- 最长公共子串长度

  允许增加、删除字符这两个编辑操作。

  最长公共子串的大小，表示两个字符串相似程度的大小。

  比如：`mitcmu`和`mtacnu`的公共最长公共子串长度是4。

## 莱文斯坦距离

### 回溯法

递归考察`a[i]`与`b[j]`，如果不匹配，可以有多种处理方式：

- 删除`a[i]`，然后递归考察`a[i+1]`和`b[j]`
- 删除`b[j]`，然后考察`a[i]`和`b[j+1]`
- 在`a[i]`前添加一个跟`b[j]`相同的字符，然后考察`a[i]`和`b[j+1]`
- 在`b[j]`前添加一个跟`a[i]`相同的字符，然后递归考察`a[i+1]`和`b[j]`
- 将`a[i]`替换成`b[j]`，或者将`b[j]`替换成`a[i]`，然后考察`a[i+1]`和`b[j+1]`

如果`a[i]`与`b[j]`匹配，则考察`a[i+1]`和`b[j+1]`

回溯解法代码：

```go
func lwstBT(str1, str2 string) int {
	minDist := len(str1)
	if len(str2) > len(str1) {
		minDist = len(str2)
	}
	return helper(str1, str2, 0, 0, 0, minDist)
}

func helper(str1, str2 string, i, j, curDist, minDist int) int {
	l1, l2 := len(str1), len(str2)
	if i == l1 || j == l2 {
		if i < l1 {
			curDist += (l1 - i)
		}
		if j < l2 {
			curDist += (l2 - j)
		}
		if curDist < minDist {
			minDist = curDist
		}
		return minDist
	}

	if str1[i] == str2[j] {
		return helper(str1, str2, i+1, j+1, curDist, minDist)
	}
	// 删除str1[i] 或者str2[j]前添加字符
	minDist = helper(str1, str2, i+1, j, curDist+1, minDist)
	// 删除str2[j] 或者str1[i]前添加字符
	minDist = helper(str1, str2, i, j+1, curDist+1, minDist)
	// 替换str1[i]或者str2[j] 是
	minDist = helper(str1, str2, i+1, j+1, curDist+1, minDist)
	return minDist
}
```

使用回溯解法，会产生很多重复状态，符合动态规划“重复子问题”的要求，所以可以使用“动态规划”来解题 。

### 动态规划

使用`min_edist`表示某一阶段决策结束之后，获得的最少编辑次数。对于`a[i]`和`b[j]`，我们可以知道：

```bash
# 如果a[i] != b[j]
min_edist(i, j) = min(
	min_edist(i-1, j) + 1,
	min_edist(i, j-1) + 1,
	min_edist(i-1, j-1) + 1,
)

# 如果a[i] == b[j]
min_edist(i, j) = min(
	min_edist(i-1, j),
	min_edist(i, j-1),
	min_edist(i-1, j-1),
)
```

将上述状态转移方程翻译成代码，可得：

```go
func lwstBT(str1, str2 string) int {
	dp := make([][]int, len(str1))
	for i := range dp {
		dp[i] = make([]int, len(str2))
	}
	if str1[0] == str2[0] {
		dp[0][0] = 0
	} else {
		dp[0][0] = 1
	}
	for i := 1; i < len(str2); i++ {
		if str1[0] == str2[i] {
			dp[0][i] = i
		} else {
			dp[0][i] = dp[0][i-1] + 1
		}
	}
	for i := 1; i < len(str1); i++ {
		if str2[0] == str1[i] {
			dp[i][0] = i
		} else {
			dp[i][0] = dp[i-1][0] + 1
		}
	}
	for i := 1; i < len(str1); i++ {
		for j := 1; j < len(str2); j++ {
			if str1[i] == str2[j] {
				dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])
			} else {
				dp[i][j] = min(dp[i-1][j-1]+1, dp[i-1][j]+1, dp[i][j-1]+1)
			}
		}
	}
	return dp[len(str1)-1][len(str2)-1]
}

func min(x, y, z int) int {
	if x > y {
		if y > z {
			return z
		}
		return y
	}
	if x > z {
		return z
	}
	return x
}
```

## 最长公共子串长度

### 回溯思路

考察`a[i]`和`b[j]`。

- 如果匹配，将公共子串长度`+1`，然后考察`a[i+1]`和`b[j+1]`
- 如果不匹配，最长公共子串长度不变。这时，可删除`a[i]`，或者在`b[j]`前加一个字符，然后考察`a[i+1]`和`b[j]`；还可以删除`b[j]`，或者在`a[i]`前加一个字符`b[j]`，然后考察`a[i]`和`b[j+1]`

### 动态规划思想

使用`max_lcs`，表示当前决策阶段的最长公共子串长度。

```bash
# 如果a[i] == b[j]
max_lcs(i, j) = max(
	max_lcs(i-1, j-1)+1, max_lcs(i-1, j)+1, max_lcs(i, j-1)+1,
)

# 如果a[i] != b[j]
max_lcs(i, j) = max(
	max_lcs(i, j-1), max_lcs(i-1, j), max_lcs(i-1, j-1),
)
```

将动态转移方程翻译成代码：

```go
func lcs(str1, str2 string) int {
	dp := make([][]int, len(str1))
	for i := range dp {
		dp[i] = make([]int, len(str2))
	}

	if str1[0] == str2[0] {
		dp[0][0] = 1
	} else {
		dp[0][0] = 0
	}
	for i := 1; i < len(str2); i++ {
		if str1[0] == str2[i] {
			dp[0][i] = 1
		} else {
			dp[0][i] = dp[0][i-1]
		}
	}
	for i := 1; i < len(str1); i++ {
		if str2[0] == str1[i] {
			dp[i][0] = 1
		} else {
			dp[i][0] = dp[i-1][0]
		}
	}
	for i := 1; i < len(str1); i++ {
		for j := 1; j < len(str2); j++ {
			if str1[i] == str2[j] {
				dp[i][j] = max(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1
			} else {
				dp[i][j] = max(dp[i-1][j-1], dp[i-1][j], dp[i][j-1])
			}
		}
	}
	return dp[len(str1)-1][len(str2)-1]
}

func max(x, y, z int) int {
	max := -1
	if x > max {
		max = x
	}
	if y > max {
		max = y
	}
	if z > max {
		max = z
	}
	return max
}
```

