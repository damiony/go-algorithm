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

	start, end := 0, -1 // 返回s[start:end+1]，所以end需要+1
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
	start, end := 0, 0
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
	// 将字符串转换成偶数个字符
	t := "#"
	for i := 0; i < len(s); i++ {
		t += string(s[i]) + "#"
	}
	arm := []int{}
	// lastMid：手臂最远时，对应的中心点
	// maxArm：最远的手臂长度
	// lastAns：最长回文串对应的中心点下标
	lastMid, maxArm, lastAns := 0, 0, 0
	for i := 0; i < len(t); i++ {
		if i < lastMid+maxArm {
			// 如果该中心点位于最远臂长的范围内
			cur := lastMid + maxArm - i
			if cur > arm[2*lastMid-i] {
				cur = arm[2*lastMid-i]
			}
			left, right := i-cur, i+cur
			for left-1 >= 0 && right+1 < len(t) && t[left-1] == t[right+1] {
				left--
				right++
			}
			cur = (right - left) >> 1
			arm = append(arm, cur)
		} else {
			// 如果该中心点已经超出最远臂长的范围
			left, right := i, i
			for left-1 >= 0 && right+1 < len(t) && t[left-1] == t[right+1] {
				left--
				right++
			}
			cur := (right - left) >> 1
			arm = append(arm, cur)
		}
		// 更新最远臂长
		if arm[i] > maxArm {
			maxArm = arm[i]
			lastMid = i
		}
		// 更新最长回文串对应的中心点
		if arm[lastMid] > arm[lastAns] {
			lastAns = lastMid
		}
	}

	start := (lastAns - arm[lastAns]) >> 1
	end := start + arm[lastAns]
	return s[start:end]
}
```

**解法四**：转换为最长公共子串

可以先将字符串翻转，再使用动态规划求解，代码暂时写不动了。