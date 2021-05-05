### 题目描述

**36进制数相加**

36进制由`0-9`，`a-z`共36个字符表示。

按照加法规则计算出任意两个36进制正整数的和。

**示例：**

```shell
输入：1b, 2x
输入: 48
```

### 题解

**解法一：按位相加**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func add36strings(str1, str2 string) string {
	res := []byte{}
	carry := 0
	i, j := len(str1)-1, len(str2)-1
	for i >= 0 || j >= 0 || carry > 0 {
		var num1, num2, sum int
		if i >= 0 {
			num1 = getNum(str1[i])
			i--
		}
		if j >= 0 {
			num2 = getNum(str2[j])
			j--
		}

		sum = num1 + num2 + carry
		carry = sum / 36
		sum = sum % 36

		res = append(res, getChar(sum))
	}

	for i := 0; i < len(res)/2; i++ {
		res[i], res[len(res)-1-i] = res[len(res)-1-i], res[i]
	}
	return string(res)
}

func getNum(b byte) int {
	if b >= '0' && b <= '9' {
		return int(b - '0')
	} else {
		return int(b-'a') + 10
	}
}

func getChar(n int) byte {
	if n >= 0 && n <= 9 {
		return byte(n + '0')
	} else {
		return byte(n - 10 + 'a')
	}
}
```