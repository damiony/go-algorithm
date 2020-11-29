最小基因变化

----

### 题目描述

一条基因序列由一个带有8个字符的字符串表示，其中每个字符都属于 "A", "C", "G", "T"中的任意一个。

假设我们要调查一个基因序列的变化。一次基因变化意味着这个基因序列中的一个字符发生了变化。

例如，基因序列由"AACCGGTT" 变化至 "AACCGGTA" 即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — start, end, bank，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。

**注意**:

起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
所有的目标基因序列必须是合法的。
假定起始基因序列与目标基因序列是不一样的。

**示例**：

```bash
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

返回值: 1
```

```bash
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

返回值: 3
```

----

### 解法

解法一：深度优先遍历

- 时间复杂度：
- 空间复杂度：

```go
func minMutation(start string, end string, bank []string) int {
	alter := []string{"A", "C", "G", "T"}
	visit := map[int]bool{}
	minCount := len(start) + 1

	var dfs func(string, int)
	dfs = func(start string, count int) {
		if start == end {
			if count < minCount {
				minCount = count
			}
			return
		}

		for i := 0; i < len(start); i++ {
			for _, v := range alter {
				if v == string(start[i]) {
					continue
				}
				tmp := start[:i] + v + start[i+1:]
				if index := find(tmp, bank); index != -1 && !visit[index] {
					visit[index] = true
					dfs(tmp, count+1)
					visit[index] = false
				}
			}
		}
	}

	dfs(start, 0)
	if minCount == len(start)+1 {
		return -1
	}
	return minCount
}

func find(str string, bank []string) int {
	for i, v := range bank {
		if v == str {
			return i
		}
	}
	return -1
}
```



解法二：单向广度优先遍历

- 时间复杂度：
- 空间复杂度：

```go
func minMutation(start string, end string, bank []string) int {
	if index := find(end, bank); index == -1 {
		return -1
	}

	alter := []string{"A", "C", "G", "T"}
	queue := []string{start}
	count := 0
	for len(queue) > 0 {
		qLen := len(queue)
		for _, str := range queue {
			if str == end {
				return count
			}

			for i := 0; i < len(str); i++ {
				for _, v := range alter {
					tmp := str[:i] + v + str[i+1:]
					if index := find(tmp, bank); index != -1 {
						bank = append(bank[0:index], bank[index+1:]...)
						queue = append(queue, tmp)
					}
				}
			}
		}
		queue = queue[qLen:]
		count++
	}
	return -1
}

func find(str string, bank []string) int {
	for i, v := range bank {
		if v == str {
			return i
		}
	}
	return -1
}
```



解法三：广度优先遍历

- 时间复杂度：
- 空间复杂度：

```go
func minMutation(start string, end string, bank []string) int {
	if index := find(end, bank); index == -1 {
		return -1
	}

	alter := []string{"A", "C", "G", "T"}
	startQ := []string{start}
	endQ := []string{end}
	count := 0
	for len(startQ) > 0 {
        count++
		startLen := len(startQ)
		for _, str := range startQ {
			for i := 0; i < len(str); i++ {
				for _, v := range alter {
					tmp := str[:i] + v + str[i+1:]
					if index := find(tmp, endQ); index != -1 {
						return count
					}
					if index := find(tmp, bank); index != -1 {
						startQ = append(startQ, tmp)
						bank = append(bank[:index], bank[index+1:]...)
					}
				}
			}
		}
		startQ = startQ[startLen:]
		if len(startQ) > len(endQ) {
			startQ, endQ = endQ, startQ
		}
	}
	return -1
}

func find(str string, bank []string) int {
	for i, v := range bank {
		if v == str {
			return i
		}
	}
	return -1
}
```

