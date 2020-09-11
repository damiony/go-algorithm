`N`叉树的层序遍历

----

### 题目描述

给定一个`N`叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）

----

### 解法

`N`叉树的定义为：

```go
type Node struct {
    Val int
    Children []*Node
}
```



解法一：队列

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func levelOrder(root *Node) [][]int {
    result := [][]int{}
    if root == nil {
        return result
    }
    queue := []*Node{root}
    index := 0

    for index < len(queue) {
        layerEnd := len(queue)
        current := []int{}

        for index < layerEnd {
            root = queue[index]
            queue = append(queue, root.Children...)
            current = append(current, root.Val)
            index++
        }

        result = append(result, current)
    }

    return result
}
```



解法二：队列优化

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func levelOrder(root *Node) [][]int {
    result := [][]int{}
    if root == nil {
        return result
    }

    stack := []*Node{root}
    for len(stack) > 0 {
        newStack := []*Node{}
        levelResult := []int{}
        for _, v := range stack {
            levelResult = append(levelResult, v.Val)
            newStack = append(newStack, v.Children...)
        }

        result = append(result, levelResult)
        stack = newStack
    }

    return result
}
```



解法三：递归

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func levelOrder(root *Node) [][]int {
    result := [][]int{}
    if root == nil {
        return result
    }

    result = layerOrder(root, result, 0)
    return result
}

func layerOrder(root *Node, result [][]int, layer int) [][]int {
    if len(result) == layer {
        result = append(result, []int{})
    }
    result[layer] = append(result[layer], root.Val)
    for _, node := range root.Children {
        result = layerOrder(node, result, layer + 1)
    }
    return result
}
```

