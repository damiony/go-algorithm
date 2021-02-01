### 题目描述

**二叉树的后序遍历：**

给定一个二叉树，返回它的后序遍历。

**示例：**

```shell
输入：[1, null, 2, 3]
输出：[3, 2, 1]
```

### 解法

`go`的二叉树定义为：

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
func postorderTraversal(root *TreeNode) []int {
    ans := []int{}
    if root == nil {
        return ans
    }
    ans = append(ans, postorderTraversal(root.Left)...)
    ans = append(ans, postorderTraversal(root.Right)...)
    ans = append(ans, root.Val)
    return ans
}
```

**解法二：迭代**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func postorderTraversal(root *TreeNode) []int {
	ans := []int{}
	stack := []*TreeNode{}
	var pre *TreeNode // 前一个记录的节点
	for root != nil || len(stack) > 0 {
		for root != nil {
			stack = append(stack, root)
			root = root.Left
		}

		index := len(stack) - 1
		node := stack[index]

		if node.Right != nil && node.Right != pre {
			root = node.Right
		} else {
			ans = append(ans, node.Val)
			stack = stack[:index]
			pre = node
		}
	}
	return ans
}
```

**解法三：两个栈**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func postorderTraversal(root *TreeNode) []int {
	ans := []int{}
	if root == nil {
		return ans
	}
	stack1 := []*TreeNode{root}
	stack2 := []*TreeNode{}
	for len(stack1) > 0 {
		index := len(stack1) - 1
		node := stack1[index]
		stack1 = stack1[:index]
		stack2 = append(stack2, node)

		if node.Left != nil {
			stack1 = append(stack1, node.Left)
		}
		if node.Right != nil {
			stack1 = append(stack1, node.Right)
		}
	}
	for i := len(stack2) - 1; i >= 0; i-- {
		ans = append(ans, stack2[i].Val)
	}
	return ans
}
```

**解法四：先序 + 翻转**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func postorderTraversal(root *TreeNode) []int {
	ans := []int{}
	stack := []*TreeNode{}
	for root != nil || len(stack) > 0 {
		for root != nil {
			ans = append(ans, root.Val)
			stack = append(stack, root)
			root = root.Right
		}

		index := len(stack) - 1
		node := stack[index]
		stack = stack[:index]
		root = node.Left
	}

	n := len(ans) - 1
	for i := 0; i <= n>>1; i++ {
		ans[i], ans[n-i] = ans[n-i], ans[i]
	}
	return ans
}
```

**解法五：morris遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func postorderTraversal(root *TreeNode) []int {
	ans := []int{}
	dummy := root
	for root != nil {
		if root.Left == nil {
			root = root.Right
			continue
		}
		leftTree := root.Left
		for leftTree.Right != nil && leftTree.Right != root {
			leftTree = leftTree.Right
		}

		if leftTree.Right == root {
			leftTree.Right = nil
			ans = reverse(root.Left, ans)
			root = root.Right
		} else {
			leftTree.Right = root
			root = root.Left
		}
	}

	ans = reverse(dummy, ans)
	return ans
}

func reverse(root *TreeNode, ans []int) []int {
	cur := []int{}
	for root != nil {
		cur = append(cur, root.Val)
		root = root.Right
	}
	for i := len(cur) - 1; i >= 0; i-- {
		ans = append(ans, cur[i])
	}
	return ans
}
```