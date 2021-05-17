### 题目描述

**二叉树的完全性检验：**

给定一个二叉树，确定它是否是一个完全二叉树。

### 解法

**解法一：广度遍历优先**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func isCompleteTree(root *TreeNode) bool {
	if root == nil {
		return true
	}
	// 最大坐标 总节点数
	max, nodes := 1, 1
	// 节点队列
	nodeQ := []*TreeNode{root}
	// 节点对应位置节点
	posQ := []int{1}
	for len(nodeQ) > 0 {
		node := nodeQ[0]
		pos := posQ[0]
		nodeQ = nodeQ[1:]
		posQ = posQ[1:]

		if node.Left != nil {
			nodeQ = append(nodeQ, node.Left)
			max = 2 * pos
			posQ = append(posQ, max)
			nodes++
		}
		if node.Right != nil {
			nodeQ = append(nodeQ, node.Right)
			max = 2*pos + 1
			posQ = append(posQ, max)
			nodes++
		}
	}
	return max == nodes
}
```