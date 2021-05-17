单词接龙

----

### 题目描述

给定两个单词（`beginWord` 和 `endWord`）和一个字典，找到从`beginWord`到`endWord`的最短转换序列的长度。转换需遵循如下规则：

1. 每次转换只能改变一个字母。

2. 转换过程中的中间单词必须是字典中的单词。

说明:

1. 如果不存在这样的转换序列，返回 0。
2. 所有单词具有相同的长度。
3. 所有单词只由小写字母组成。
4. 字典中不存在重复的单词。
5. 你可以假设`beginWord`和`endWord`是非空的，且二者不相同。

示例：

```bash
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5
```

----

### 解法

解法一：单向广度遍历

- 时间复杂度：`O(N*C)`
- 空间复杂度：`O(N)`

```go
func ladderLength(beginWord string, endWord string, wordList []string) int {
	// 边界条件
	if beginWord == endWord {
		return 1
	}
	// slice 转 map，用于快速查找
	wordMap := map[string]bool{}
	for _, w := range wordList {
		wordMap[w] = true
	}
	if _, ok := wordMap[endWord]; !ok {
		return 0
	}
	// 转换次数
	count := 0
	// 存储转换得到的开始字符串
	startQueue := []string{beginWord}
	// 具体的bfs算法
	for len(startQueue) > 0 {
		q := startQueue
		startQueue = nil

		count++
		startMap := map[string]bool{}
		for _, str := range q {
			for i := 0; i < len(str); i++ {
				for j := 'a'; j <= 'z'; j++ {
					tmp := str[:i] + string(j) + str[i+1:]
					if tmp == endWord {
						return count + 1
					}
					if _, ok := wordMap[tmp]; ok {
						startMap[tmp] = true
						startQueue = append(startQueue, tmp)
						delete(wordMap, tmp)
					}
				}
			}
		}
	}
	return 0
}
```



解法二：双向广度遍历

- 时间复杂度：`O(N*C)`
- 空间复杂度：`O(N)`

```go
func ladderLength(beginWord string, endWord string, wordList []string) int {
	// 边界条件
	if beginWord == endWord {
		return 1
	}
	// slice 转 map，用于快速查找
	wordMap := map[string]bool{}
	for _, w := range wordList {
		wordMap[w] = true
	}
	if _, ok := wordMap[endWord]; !ok {
		return 0
	}
	// 转换次数
	count := 0
	// 存储转换得到的开始字符串
	startQueue := []string{beginWord}
	// 存储转换得到的结束字符串
	endQueue := []string{endWord}
	// 存储转换得到的结束字符串 用于快速查找
	endMap := map[string]bool{endWord: true}
	// 具体的bfs算法
	for len(startQueue) > 0 {
		q := startQueue
		startQueue = nil

		count++
		startMap := map[string]bool{}
		for _, str := range q {
			for i := 0; i < len(str); i++ {
				for j := 'a'; j <= 'z'; j++ {
					tmp := str[:i] + string(j) + str[i+1:]
					if _, ok := endMap[tmp]; ok {
						return count + 1
					}
					if _, ok := wordMap[tmp]; ok {
						startMap[tmp] = true
						startQueue = append(startQueue, tmp)
						delete(wordMap, tmp)
					}
				}
			}
		}
		if len(startQueue) > len(endQueue) {
			startQueue, endQueue = endQueue, startQueue
			endMap = startMap
		}
	}
	return 0
}
```



解法三：深度优先搜索

解法很简单，但是复杂度较高，暂时只想了个思路，代码省略。