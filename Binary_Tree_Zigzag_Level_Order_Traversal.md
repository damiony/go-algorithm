### 题目描述

**二叉树的锯齿形层序遍历：**

给定一个二叉树，返回其节点值的锯齿形层序遍历。

即先从左向右遍历，再从右往左进行下一层遍历。

**示例：**

```shell
输入：[3,9,20,null,null,15,7]
    3
   / \
  9  20
    /  \
   15   7


输出：[
        [3],
        [20,9],
        [15,7]
      ]
```

### 解法

二叉树节点的定义为：

```go
type TreeNode struct {
  Val int
  Left *TreeNode
  Right *TreeNode
}
```

**解法一：广度优先遍历**

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func zigzagLevelOrder(root *TreeNode) [][]int {
	if root == nil {
		return nil
	}

	desc := false
	ans := [][]int{}
	stack := []*TreeNode{root}
	for len(stack) > 0 {
		cur := []int{}
		n := len(stack)
		for i := n - 1; i >= 0; i-- {
			node := stack[i]
			cur = append(cur, node.Val)
			if !desc { // 顺序
				if node.Left != nil {
					stack = append(stack, node.Left)
				}
				if node.Right != nil {
					stack = append(stack, node.Right)
				}
			} else { // 倒序
				if node.Right != nil {
					stack = append(stack, node.Right)
				}
				if node.Left != nil {
					stack = append(stack, node.Left)
				}
			}
		}
		stack = stack[n:]
		ans = append(ans, cur)
		desc = !desc
	}
	return ans
}
```

**解法二：深度优先遍历**

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func zigzagLevelOrder(root *TreeNode) [][]int {
	ans := [][]int{}
	return dfs(root, 1, ans)
}

func dfs(root *TreeNode, layer int, ans [][]int) [][]int {
	if root == nil {
		return ans
	}
	if len(ans) < layer {
		ans = append(ans, []int{})
	}
	if layer&1 == 1 {
		ans[layer-1] = append(ans[layer-1], root.Val)
	} else {
		ans[layer-1] = append([]int{root.Val}, ans[layer-1]...)
	}
	ans = dfs(root.Left, layer+1, ans)
	ans = dfs(root.Right, layer+1, ans)
	return ans
}
```