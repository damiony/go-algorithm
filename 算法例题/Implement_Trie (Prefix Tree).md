实现Trie（前缀树）

----

### 题目描述

实现一个`trie`，包含`insert`、`search`、`startsWith`这三个操作。

- 所有输入都是由小写字母`a-z`构成。
- 所有输入均为非空字符。

示例：

```bash
trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

----

### 解法

解法一：使用哈希表

```go
type Trie struct {
	children map[byte]*Trie
	end      bool
}

/** Initialize your data structure here. */
func Constructor() Trie {
	return Trie{
		children: map[byte]*Trie{},
	}
}

/** Inserts a word into the trie. */
func (this *Trie) Insert(word string) {
	node := this
	for i := 0; i < len(word); i++ {
		if node.children[word[i]] == nil {
			node.children[word[i]] = &Trie{
				children: map[byte]*Trie{},
			}
		}
		node = node.children[word[i]]
	}
	node.end = true
	return
}

/** Returns if the word is in the trie. */
func (this *Trie) Search(word string) bool {
	node := this
	for i := 0; i < len(word); i++ {
		if node.children[word[i]] == nil {
			return false
		}
		node = node.children[word[i]]
	}
	return node.end
}

/** Returns if there is any word in the trie that starts with the given prefix. */
func (this *Trie) StartsWith(prefix string) bool {
	node := this
	for i := 0; i < len(prefix); i++ {
		if node.children[prefix[i]] == nil {
			return false
		}
		node = node.children[prefix[i]]
	}
	return true
}

/**
 * Your Trie object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Insert(word);
 * param_2 := obj.Search(word);
 * param_3 := obj.StartsWith(prefix);
 */
```

