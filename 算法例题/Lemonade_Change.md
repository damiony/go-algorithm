柠檬水找零

----

### 题目描述

一杯柠檬水5元。现在有一批顾客，每人排队购买一杯，支付的钱可能为`5`元、`10`元、`20`元。请问能不能正确找零。

示例1：

```bash
输入：[5, 5, 5, 10, 20]
输出：true
```

示例2：

```bash
输入：[5, 5, 10, 10, 20]
输出：false
```

----

### 解法

解法一：贪心算法

- 时间复杂度：`O(N)`
- 空间复杂度：`O(1)`

```go
func lemonadeChange(bills []int) bool {
	five, ten := 0, 0
	for _, money := range bills {
		if money == 5 {
			five++
		} else if money == 10 {
			if five <= 0 {
				return false
			}
			five--
			ten++
		} else if money == 20 {
			if ten <= 0 {
				if five < 3 {
					return false
				}
				five -= 3
			} else {
				if five < 1 {
					return false
				}
				five--
				ten--
			}
		}
	}
	return true
}
```

