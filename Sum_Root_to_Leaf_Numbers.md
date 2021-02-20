### 题目描述

**求根节点到叶子节点数字之和：**

给定一个二叉树，它的每个结点都存放一个`0-9`的数字，每条从根到叶子节点的路径都代表一个数字。

例如，从根到叶子节点路径`1->2->3`代表数字`123`。

计算从根到叶子节点生成的所有数字之和。

### 解法

**解法一：深度遍历优先**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func sumNumbers(root *TreeNode) int {
	if root == nil {
		return 0
	}
	return helper(root, root.Val)
}

func helper(root *TreeNode, cur int) int {
	if root.Left == nil && root.Right == nil {
		return cur
	}

	var leftNum, rigNum int
	if root.Left != nil {
		leftNum = helper(root.Left, cur*10+root.Left.Val)
	}
	if root.Right != nil {
		rigNum = helper(root.Right, cur*10+root.Right.Val)
	}

	return leftNum + rigNum
}
```

**解法二：广度优先遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func sumNumbers(root *TreeNode) int {
	if root == nil {
		return 0
	}

	qNode := []*TreeNode{root} // 节点队列
	qNum := []int{root.Val}    // 节点对应数值的队列
	ans := 0                   // 值的和

	for len(qNode) > 0 {
		qLen := len(qNode)
		for i := 0; i < qLen; i++ {
			node := qNode[i]
			num := qNum[i]
			if node.Left != nil {
				qNum = append(qNum, 10*num+node.Left.Val)
				qNode = append(qNode, node.Left)
			}
			if node.Right != nil {
				qNum = append(qNum, 10*num+node.Right.Val)
				qNode = append(qNode, node.Right)
			}
			if node.Left == nil && node.Right == nil {
				ans += num
			}
		}
		qNode = qNode[qLen:]
		qNum = qNum[qLen:]
	}

	return ans
}
```