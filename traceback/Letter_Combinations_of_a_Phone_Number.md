电话号码的字母组合

----

### 题目描述

给定一个仅包含数字`2-9`的字符串，返回所有它能表示的字母组合。

数字到字母的映射如下：

- 2: a b c
- 3: d e f
- 4: g h i
- 5: j k l
- 6: m n o
- 7: p q r s
- 8: t u v
- 9: w x y z

示例：

```shell
输入："23"
输出：["ab", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
```

----

### 解法：

解法一：BFS + 迭代

- 时间复杂度：`O(3^m * 4^n)`，`m + n`是字符串长度
- 空间复杂度：`O(1)`

```go
func letterCombinations(digits string) []string {
	if len(digits) == 0 {
		return []string{}
	}

	numMap := map[byte][]string{
		'2': {"a", "b", "c"},
		'3': {"d", "e", "f"},
		'4': {"g", "h", "i"},
		'5': {"j", "k", "l"},
		'6': {"m", "n", "o"},
		'7': {"p", "q", "r", "s"},
		'8': {"t", "u", "v"},
		'9': {"w", "x", "y", "z"},
	}

	result := []string{""}
	for _, num := range []byte(digits) {
		if strings, ok := numMap[num]; ok {
			newResult := []string{}
			for _, s := range strings {
				for _, ele := range result {
					newResult = append(newResult, ele+s)
				}
			}
			result = newResult
		}
	}
	return result
}
```



解法二：BFS + 递归

- 时间复杂度：`O(3^m * 4^n)`，`m + n`是字符串长度。
- 空间复杂度：`O(m + n)`，递归深度

```go
func letterCombinations(digits string) []string {
	if len(digits) == 0 {
		return []string{}
	}

	numMap := map[byte][]string{
		'2': {"a", "b", "c"},
		'3': {"d", "e", "f"},
		'4': {"g", "h", "i"},
		'5': {"j", "k", "l"},
		'6': {"m", "n", "o"},
		'7': {"p", "q", "r", "s"},
		'8': {"t", "u", "v"},
		'9': {"w", "x", "y", "z"},
	}

	queue := []string{""}
	return recur(digits, queue, numMap)
}

func recur(digits string, result []string, numMap map[byte][]string) []string {
	if len(digits) == 0 {
		return result
	}

	newResult := []string{}
	if strings, ok := numMap[digits[0]]; ok {
		for _, s := range strings {
			for _, ele := range result {
				newResult = append(newResult, ele+s)
			}
		}
	}

	return recur(digits[1:], newResult, numMap)
}
```



解法三：回溯

- 时间复杂度：`O(3^m * 4^n)`，`m + n`是字符串长度。
- 空间复杂度：`O(m + n)`

```go
func letterCombinations(digits string) []string {
	if len(digits) == 0 {
		return []string{}
	}

	numMap := map[string]string{
		"2": "abc",
		"3": "def",
		"4": "ghi",
		"5": "jkl",
		"6": "mno",
		"7": "pqrs",
		"8": "tuv",
		"9": "wxyz",
	}
	result := []string{}
	return traceback(result, digits, "", numMap)
}

func traceback(result []string, digits string, str string, numMap map[string]string) []string {
	index := len(str)
	if index == len(digits) {
		result = append(result, str)
		return result
	}

	digit := string(digits[index])
	if numToString, ok := numMap[digit]; ok {
		for i := 0; i < len(numToString); i++ {
			ele := string(numToString[i])
			result = traceback(result, digits, str+ele, numMap)
		}
	}
	return result
}
```

