验证二叉搜索树

----

### 题目描述

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

二叉搜索树的定义：

> 节点的左子树只包含小于当前节点的数。
>
> 节点的右子树只包含大于当前节点的数。
>
> 所有左子树和右子树自身必须也是二叉搜索树。

----

### 解法

二叉树的定义为：

```go
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}
```



解法一：递归

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func isValidBST(root *TreeNode) bool {
	return isBst(root, math.MinInt64, math.MaxInt64)
}

func isBst(root *TreeNode, min int, max int) bool {
	if root == nil {
		return true
	}
	if root.Val <= min || root.Val >= max {
		return false
	}
	return isBst(root.Left, min, root.Val) && isBst(root.Right, root.Val, max)
}
```



解法二：迭代+栈

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func isValidBST(root *TreeNode) bool {
	if root == nil {
		return true
	}

	minQ := []int{math.MinInt64}
	maxQ := []int{math.MaxInt64}
	stack := []*TreeNode{root}
	for len(stack) > 0 {
		index := len(stack) - 1
		node, min, max := stack[index], minQ[index], maxQ[index]
		stack, minQ, maxQ = stack[:index], minQ[:index], maxQ[:index]

		if node.Val <= min || node.Val >= max {
			return false
		}
		if node.Left != nil {
			stack = append(stack, node.Left)
			minQ = append(minQ, min)
			maxQ = append(maxQ, node.Val)
		}
		if node.Right != nil {
			stack = append(stack, node.Right)
			minQ = append(minQ, node.Val)
			maxQ = append(maxQ, max)
		}
	}
	return true
}
```



解法三：迭代+队列

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func isValidBST(root *TreeNode) bool {
	if root == nil {
		return true
	}

	minQ := []int{math.MinInt64}
	maxQ := []int{math.MaxInt64}
	stack := []*TreeNode{root}
	for len(stack) > 0 {
		node, min, max := stack[0], minQ[0], maxQ[0]
		stack, minQ, maxQ = stack[1:], minQ[1:], maxQ[1:]

		if node.Val <= min || node.Val >= max {
			return false
		}
		if node.Left != nil {
			stack = append(stack, node.Left)
			minQ = append(minQ, min)
			maxQ = append(maxQ, node.Val)
		}
		if node.Right != nil {
			stack = append(stack, node.Right)
			minQ = append(minQ, node.Val)
			maxQ = append(maxQ, max)
		}
	}
	return true
}
```



解法四：中序遍历

二叉搜索树的中序遍历结果是有序的

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func isValidBST(root *TreeNode) bool {
	pre := &TreeNode{Val: math.MinInt64}
	stack := []*TreeNode{}
	for len(stack) > 0 || root != nil {
		for root != nil {
			stack = append(stack, root)
			root = root.Left
		}
		index := len(stack) - 1
		if stack[index].Val <= pre.Val {
			return false
		}
		pre = stack[index]
		stack = stack[:index]
		root = pre.Right
	}
	return true
}
```

