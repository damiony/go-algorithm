### 题目描述

给定一个`N`叉树，返回其节点值的后序遍历。

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
func postorder(root *Node) []int {
    result := []int{}
    if root == nil {
        return result
    }
    for i := 0; i < len(root.Children); i++ {
        result = append(result, postorder(root.Children[i])...)
    }
    result = append(result, root.Val)
    return result
}
```



解法二：迭代法：

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func postorder(root *Node) []int {
    result := []int{}
    if root == nil {
        return result
    }

    stack1 := []*Node{root}
    stack2 := []int{}
    for len(stack1) > 0 {
        index := len(stack1) - 1
        root := stack1[index]
        stack1 = stack1[:index]

        stack2 = append(stack2, root.Val)
        for i := 0; i < len(root.Children); i++ {
            if root.Children[i] != nil {
                stack1 = append(stack1, root.Children[i])
            }
        }
    }

    for i := len(stack2) - 1; i >= 0; i-- {
        result = append(result, stack2[i])
    }
    return result
}
```



