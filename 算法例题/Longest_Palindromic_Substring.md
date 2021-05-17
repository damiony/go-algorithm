### 题目描述

**最长回文子串**

给定一个字符串`s`，找到`s`中最长的回文串。

**示例**

```bash
输入："babad"
输出："bab"
```

### 解法

**解法一**：动态规划

- 时间复杂度：`O(n ^ 2)`
- 空间复杂度：`O(n ^ 2)`

```go
func longestPalindrome(s string) string {
	n := len(s)
	if n <= 1 {
		return s
	}
	dp := make([][]bool, n)
	for i := range dp {
		dp[i] = make([]bool, n)
	}

	start, end := 0, -1 // 返回s[start:end+1]
	for w := 0; w < n; w++ {
		for left := 0; left+w < n; left++ {
			right := left + w
			if s[left] == s[right] {
				if left == right || left+1 == right {
					dp[left][right] = true
				} else {
					dp[left][right] = dp[left+1][right-1]
				}
			}
			if dp[left][right] && right-left > end-start {
				start, end = left, right
			}
		}
	}
	return s[start : end+1]
}
```

**解法二**：中心扩散

- 时间复杂度：`O(n^2)`
- 空间复杂度：`O(1)`

```go
func longestPalindrome(s string) string {
	n := len(s)
	if n <= 1 {
		return s
	}
	start, end := -1, 0
	for i := 0; i < n; i++ {
		left1, right1 := i, i
		for left1 >= 0 && right1 < n && s[left1] == s[right1] {
			left1--
			right1++
		}
		left2, right2 := i, i+1
		for left2 >= 0 && right2 < n && s[left2] == s[right2] {
			left2--
			right2++
		}
		if right1-left1 > end-start {
			start, end = left1, right1
		}
		if right2-left2 > end-start {
			start, end = left2, right2
		}
	}
	return s[start+1:end]
}
```

**解法三**：Manacher算法

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func longestPalindrome(s string) string {
	n := len(s)
	if n <= 1 {
		return s
	}
	// 奇数长度转换成偶数长度
	t := "#"
	for i := 0; i < n; i++ {
		t += string(s[i]) + "#"
	}
	n = len(t)

	// 所有字符的臂长
	arm := make([]int, n)
	// 中心点 臂长 最长臂长对应的中心
	mid, armL, resM := 0, 0, 0

	for i := 0; i < n; i++ {
		if i < mid+armL {
			// 先计算最小边界
			cur := mid + armL - i
			if cur > arm[2*mid-i] {
				cur = arm[2*mid-i]
			}
			// 根据边界扩散
			left, right := i-cur, i+cur
			for left-1 >= 0 && right+1 < n && t[left-1] == t[right+1] {
				left--
				right++
			}
			// 记录臂长
			arm[i] = (right - left) >> 1
		} else {
			left, right := i, i
			for left-1 >= 0 && right+1 < n && t[left-1] == t[right+1] {
				left--
				right++
			}
			// 记录臂长
			arm[i] = (right - left) >> 1
		}
		// 更新臂长范围
		if mid+armL < i+arm[i] {
			mid, armL = i, arm[i]
		}
		// 更新答案
		if arm[i] > arm[resM] {
			resM = i
		}
	}

	start := (resM - arm[resM]) >> 1
	end := start + arm[resM]
	return s[start:end]
}
```

**解法四**：转换为最长公共子串

可以先将字符串翻转，再使用动态规划求解，代码暂时写不动了。