二叉树的层序遍历

----

### 题目描述

给你一个二叉树，返回其按层序遍历得到的节点值。（即逐层地，从左到右访问所有节点）

示例：

```bash
输入：[3, 9, 20, null, null, 15, 7]
输出：
[
	[3],
	[9, 20],
	[15, 7]
]
```

----

### 解法

`go`语言中二叉树地定义为：

```go
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}
```



解法一：BFS + 迭代

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func levelOrder(root *TreeNode) [][]int {
	result := [][]int{}
	if root == nil {
		return result
	}

	queue := []*TreeNode{root}
	for len(queue) > 0 {
		tmp := []int{}
		length := len(queue)
		for i := 0; i < length; i++ {
			node := queue[0]
			queue = queue[1:]
			tmp = append(tmp, node.Val)
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		result = append(result, tmp)
	}

	return result
}

```



解法二：BFS + 递归

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func levelOrder(root *TreeNode) [][]int {
	res := [][]int{}
	if root == nil {
		return res
	}

	q := []*TreeNode{root}
	return helper(res, q)
}

func helper(res [][]int, q []*TreeNode) [][]int {
	qLen := len(q)
	if qLen <= 0 {
		return res
	}

	tmp := []int{}
	for i := 0; i < qLen; i++ {
		node := q[0]
		q = q[1:]
		tmp = append(tmp, node.Val)
		if node.Left != nil {
			q = append(q, node.Left)
		}
		if node.Right != nil {
			q = append(q, node.Right)
		}
	}
	res = append(res, tmp)

	return helper(res, q)
}
```

