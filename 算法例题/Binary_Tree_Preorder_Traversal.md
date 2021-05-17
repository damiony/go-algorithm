### 题目描述

**二叉树的前序遍历：**

给定一个二叉树，返回它的前序遍历

**示例：**

```shell
输入：[1, null, 2, 3]
输出：[1, 2, 3]
```

### 解法

`go`语言中的二叉树表示：

```go
type TreeNode struct {
    Val int
    Left *TreeNode
    Right *TreeNode
}
```

**解法一：递归法**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func preorderTraversal(root *TreeNode) []int {
    result := []int{}
    if root == nil {
        return result
    }
    result = append(result, root.Val)
    result = append(result, preorderTraversal(root.Left)...)
    result = append(result, preorderTraversal(root.Right)...)
    return result
}
```

**解法二：迭代法 + 深度优先遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func preorderTraversal(root *TreeNode) []int {
    result := []int{}
    if root == nil {
        return result
    }
    stack := []*TreeNode{}
    for len(stack) > 0 || root != nil {
        for root != nil {
            result = append(result, root.Val)
            stack = append(stack, root)
            root = root.Left
        }
        index := len(stack)
        node := stack[index-1]
        stack = stack[:index-1]
        root = node.Right
    }
    return result
}
```

**解法三：迭代法 + 广度优先遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func preorderTraversal(root *TreeNode) []int {
    result := []int{}
    if root == nil {
        return result
    }
    stack := []*TreeNode{root}
    for len(stack) > 0 {
        index := len(stack)-1
        node := stack[index]
        stack = stack[:index]

        result = append(result, node.Val)
        if node.Right != nil {
            stack = append(stack, node.Right)
        }
        if node.Left != nil {
            stack = append(stack, node.Left)
        }
    }
    return result
}
```

**解法四：morris遍历**

- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`

```go
func preorderTraversal(root *TreeNode) []int {
    result := []int{}
    if root == nil {
        return result
    }
    for root != nil {
        if root.Left == nil {
            result = append(result, root.Val)
            root = root.Right
            continue
        }

        subTree := root.Left
        for subTree.Right != nil && subTree.Right != root {
            subTree = subTree.Right
        }

        if subTree.Right == root {
            root = root.Right
            subTree.Right = nil
        } else {
            subTree.Right = root
            result = append(result, root.Val)
            root = root.Left
        }
    }
    return result
}
```
