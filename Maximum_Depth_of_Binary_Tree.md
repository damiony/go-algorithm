### 题目描述

**二叉树的最大深度：**

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**示例：**

```shell
输入：[3, 9, 20, null, null, 15, 7]
输出：3
```

### 解法

**二叉树的定义为：**

```go
type TreeNode struct {
  Val int
  Left *TreeNode
  Right *TreeNode
}
```

**解法一：递归**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)` 

```shell
func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	left := maxDepth(root.Left) + 1
	right := maxDepth(root.Right) + 1
	if left > right {
		return left
	}
	return right
}
```

**解法二：`DFS`**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}

	max := 0
	level := []int{1}
	stack := []*TreeNode{root}
	for len(stack) > 0 {
		index := len(stack) - 1
		layer, node := level[index], stack[index]
		level, stack = level[:index], stack[:index]
		if max < layer {
			max = layer
		}
		if node.Left != nil {
			stack = append(stack, node.Left)
			level = append(level, layer+1)
		}
		if node.Right != nil {
			stack = append(stack, node.Right)
			level = append(level, layer+1)
		}
	}
	return max
}
```

**解法三：`BFS`**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func maxDepth(root *TreeNode) int {
	dep := 0
	if root == nil {
		return dep
	}

	queue := []*TreeNode{root}
	for len(queue) > 0 {
		dep++
		qLen := len(queue)
		for i := 0; i < qLen; i++ {
			node := queue[i]
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		queue = queue[qLen:]
	}

	return dep
}
```
