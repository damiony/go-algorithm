### 题目描述

**字符串转换整数：**

请实现一个`myAtoi(string s)`函数，使其能将字符串转换成一个`32`位有符号整数。

函数`myAtoi(string s)`的算法如下：

- 读入字符串并丢弃无用的前导空格
- 检查第一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
- 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
- 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
- 如果整数数超过 32 位有符号整数范 [−2^31,  2^31 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于`−2^31`的整数应该被固定为−2^31`，大于`2^31 − 1`的整数应该被固定为`2^31 − 1`。
- 返回整数作为最终结果。

注意：

- 本题中的空白字符只包括空格字符 ' ' 。
- 除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。

**示例：**

```shell
输入：str = "42"
输出：41
```

### 解法

**解法一：状态机**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
const (
	SPACE uint8 = iota
	SIGN
	NUMBER
	END
)

func strToInt(str string) int {
	// input: space  [+-]  [0-9]  other
	status := make([][]uint8, 4)
	status[0] = []uint8{SPACE, SIGN, NUMBER, END} // SPACE
	status[1] = []uint8{END, END, NUMBER, END}    // SIGN
	status[2] = []uint8{END, END, NUMBER, END}    // NUMBER
	status[3] = []uint8{END, END, END, END}       // END

	ans, sign := 0, 1
	cur := SPACE
	for i := 0; i < len(str); i++ {
		switch str[i] {
		case ' ':
			cur = status[cur][0]
		case '+', '-':
			cur = status[cur][1]
		case '0', '1', '2', '3', '4', '5', '6', '7', '8', '9':
			cur = status[cur][2]
		default:
			cur = status[cur][3]
		}

		if cur == END {
			break
		}
		if cur == NUMBER {
			ans = ans*10 + int(str[i]-'0')
			if ans > math.MaxInt32 + 1 {
				ans = math.MaxInt32 + 1
			}
		}
		if cur == SIGN && str[i] == '-' {
			sign = -1
		}
	}
	ans = ans * sign
	if ans > math.MaxInt32 {
		return math.MaxInt32
	}
	if ans < math.MinInt32 {
		return math.MinInt32
	}
	return ans
}
```

**解法二：循环遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func strToInt(str string) int {
	start := 0
	for ; start < len(str); start++ {
		if str[start] != ' ' {
			break
		}
	}
	if start >= len(str) {
		return 0
	}

	ans, sign := 0, 1
	if str[start] == '-' || str[start] == '+' {
		if str[start] == '-' {
			sign = -1
		}
		start++
	}
	for i := start; i < len(str); i++ {
		if str[i] < '0' || str[i] > '9' {
			break
		}
		ans = ans*10 + int(str[i]-'0')
		if ans > math.MaxInt32+1 {
			ans = math.MaxInt32 + 1
		}
	}

	ans *= sign
	if ans > math.MaxInt32 {
		return math.MaxInt32
	}
	return ans
}
```

**解法三：正则表达式**

```go
func myAtoi(s string) int {
	regStr := `^[\+\-]?\d+`
	reg := regexp.MustCompile(regStr)
	res := reg.FindString(strings.TrimLeft(s, " "))
	num, _ := strconv.Atoi(res)
	if num < math.MinInt32 {
		return math.MinInt32
	}
	if num > math.MaxInt32 {
		return math.MaxInt32
	}
	return num
}
```