### 题目描述

给定一个`N`叉树，返回其节点值的前序遍历。

----

### 解法

`N`叉树的定义为：

```go
type Node struct {
    Val int
    Children []*Node
}
```



解法一：递归

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func preorder(root *Node) []int {
    result := []int{}
    if root == nil {
        return result
    }
    result = append(result, root.Val)
    for _, v := range root.Children {
        result = append(result, preorder(v)...)
    }
    return result
}
```



解法二：迭代

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func preorder(root *Node) []int {
    result := []int{}
    if root == nil {
        return result
    }
    stack := []*Node{root}
    for len(stack) > 0 {
        index := len(stack) - 1
        root = stack[index]
        stack = stack[:index]

        result = append(result, root.Val)
        for i := len(root.Children) - 1; i >= 0; i-- {
            if root.Children[i] != nil {
                stack = append(stack, root.Children[i])
            }
        }
    }
    return result
}
```

