### 题目描述

翻转一颗二叉树

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
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return root
    }

    right, left := root.Right, root.Left
    root.Right = invertTree(left)
    root.Left = invertTree(right)

    return root
}
```



解法二：迭代

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func invertTree(root *TreeNode) *TreeNode {
    if root == nil {
        return nil
    }

    queue := []*TreeNode{root}
    for len(queue) > 0 {
        if queue[0].Left != nil {
            queue = append(queue, queue[0].Left)
        }
        if queue[0].Right != nil {
            queue = append(queue, queue[0].Right)
        }
        queue[0].Left, queue[0].Right = queue[0].Right, queue[0].Left
        queue = queue[1:]
    }

    return root
}
```

