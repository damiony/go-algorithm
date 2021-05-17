单词接龙2

----

### 题目描述

给定两个单词（beginWord 和 endWord）和一个字典 wordList，找出所有从 beginWord 到 endWord 的最短转换序列。转换需遵循如下规则：

1. 每次转换只能改变一个字母。
2. 转换后得到的单词必须是字典中的单词。

说明：

1. 如果不存在这样的转换序列，返回一个空列表。
2. 所有单词具有相同的长度。
3. 所有单词只由小写字母组成。
4. 字典中不存在重复的单词。
5. 你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

示例：

```bash
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

----

### 解法

解法一：广度遍历优先

```go
func findLadders(beginWord string, endWord string, wordList []string) [][]string {
	wordMap := map[string]bool{}
	dist := map[string][]string{}
	for _, word := range wordList {
		wordMap[word] = true
		for i := range word {
			w := word[:i] + " " + word[i+1:]
			dist[w] = append(dist[w], word)
		}
	}

	ans := [][]string{}
	if !wordMap[endWord] {
		return ans
	}

	stop := false
	queue := [][]string{}
	queue = append(queue, []string{beginWord})
	for len(queue) > 0 {
		q := queue
		queue = nil
		tmp := map[string]bool{}
		for _, list := range q {
			str := list[len(list)-1]
			for i := range str {
				w := str[:i] + " " + str[i+1:]
				for _, v := range dist[w] {
					if v == endWord {
						p := make([]string, len(list))
						copy(p, list)
						ans = append(ans, append(p, v))
						stop = true
						continue
					}
					if _, ok := wordMap[v]; !ok {
						continue
					}
					if !stop {
						l := make([]string, len(list))
						copy(l, list)
						queue = append(queue, append(l, v))
						tmp[v] = true
					}
				}
			}
		}
		if stop {
			break
		}
		for str := range tmp {
			delete(wordMap, str)
		}
	}
	return ans
}
```

