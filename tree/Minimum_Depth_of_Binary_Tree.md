二叉树的最小深度

----

### 题目描述

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

示例1：

```shell
输入：[3, 9, 20, null, null, 15, 7]
输出：2
```

示例2：

```shell
输入：[1, 2]
输出：2
```

----

### 解法

解法一：递归

- 时间复杂度：`O(n)`
- 空间复杂度：最好`O(logn)`、最坏`O(n)`

```go
func minDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}

	left := minDepth(root.Left) + 1
	right := minDepth(root.Right) + 1

	if root.Left == nil && root.Right != nil {
		return right
	}
	if root.Right == nil && root.Left != nil {
		return left
	}

	if left < right {
		return left
	}
	return right
}
```



解法二：`BFS`

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func minDepth(root *TreeNode) int {
    if root == nil {
        return 0
    }

    layer := 0
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        layer++
        for i, size := 0, len(queue); i < size; i++ {
            node := queue[0]
            queue = queue[1:]
            if node.Left == nil && node.Right == nil {
                return layer
            }
            if node.Left != nil {
                queue = append(queue, node.Left)
            }
            if node.Right != nil {
                queue = append(queue, node.Right)
            }
        }
    }
    return layer
}
```

