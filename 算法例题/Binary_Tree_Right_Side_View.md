### 题目描述

**二叉树的右视图：**

给定一颗二叉树，按照从顶部到底部的顺序，返回从右侧能看到的节点值。

### 解法

**解法一：深度优先遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func rightSideView(root *TreeNode) []int {
	ans := []int{}
	layer := 1
	return dfs(root, layer, ans)
}

func dfs(root *TreeNode, layer int, res []int) []int {
	if root == nil {
		return res
	}

	if len(res) < layer {
		res = append(res, root.Val)
	}

	res = dfs(root.Right, layer+1, res)
	res = dfs(root.Left, layer+1, res)
	return res
}
```

**解法二：广度遍历优先**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func rightSideView(root *TreeNode) []int {
	ans := []int{}
	if root == nil {
		return ans
	}

	queue := []*TreeNode{root}
	for len(queue) > 0 {
		layerL := len(queue)
		for i := 0; i < layerL; i++ {
			node := queue[i]
			if node.Left != nil {
				queue = append(queue, node.Left)
			}
			if node.Right != nil {
				queue = append(queue, node.Right)
			}
		}
		if layerL >= 1 {
			ans = append(ans, queue[layerL-1].Val)
		}
		queue = queue[layerL:]
	}

	return ans
}
```