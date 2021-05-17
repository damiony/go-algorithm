### 题目描述

**翻转二叉树：**

翻转一颗二叉树

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

```go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}
	root.Left, root.Right = invertTree(root.Right), invertTree(root.Left)
	return root
}
```

**解法二：迭代**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func invertTree(root *TreeNode) *TreeNode {
	if root == nil {
		return nil
	}
	queue := []*TreeNode{root}
	for len(queue) > 0 {
		node := queue[0]
		queue = queue[1:]
		node.Left, node.Right = node.Right, node.Left
		if node.Left != nil {
			queue = append(queue, node.Left)
		}
		if node.Right != nil {
			queue = append(queue, node.Right)
		}
	}
	return root
}

```
