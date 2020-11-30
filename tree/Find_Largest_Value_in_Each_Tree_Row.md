在每个树行中找最大值

----

### 题目描述

在二叉树的每一行中找到最大值。

示例：

```bash
输入: 
          1
         / \
        3   2
       / \   \  
      5   3   9 

输出: [1, 3, 9]
```

----

### 解法

二叉树的定义为：

```go
type TreeNode struct {
    Val int
    Left, Right *TreeNode
}
```



解法一：广度优先遍历

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func largestValues(root *TreeNode) []int {
	result := []int{}
	if root == nil {
		return result
	}

	queue := []*TreeNode{root}
	for len(queue) > 0 {
		maxValue := math.MinInt64
		qLen := len(queue)
		for _, ele := range queue {
			if ele.Val > maxValue {
				maxValue = ele.Val
			}
			if ele.Left != nil {
				queue = append(queue, ele.Left)
			}
			if ele.Right != nil {
				queue = append(queue, ele.Right)
			}
		}
		result = append(result, maxValue)
		queue = queue[qLen:]
	}

	return result
}
```



解法二：深度优先遍历（前序遍历）

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func largestValues(root *TreeNode) []int {
	result := []int{}
	return helper(root, result, 0)
}

func helper(node *TreeNode, result []int, layer int) []int {
	if node == nil {
		return result
	}

	if len(result) == layer {
		result = append(result, node.Val)
	} else {
		if result[layer] < node.Val {
			result[layer] = node.Val
		}
	}

	result = helper(node.Left, result, layer+1)
	result = helper(node.Right, result, layer+1)
	return result
}
```

