从前序与中序遍历序列构造二叉树

----

### 题目描述

根据一颗树的前序遍历与中序遍历构造二叉树。

----

### 解法

解法一：递归解法

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    root := &TreeNode{
        Val: preorder[0],
    }
    var inLen int
    for i, v := range inorder {
        if v == preorder[0] {
            inLen = i
        }
    }
    root.Left = buildTree(preorder[1:inLen+1], inorder[0:inLen])
    root.Right = buildTree(preorder[inLen+1:], inorder[inLen+1:])
    return root
}
```



解法二：迭代解法

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
    if len(preorder) == 0 {
        return nil
    }
    index := 0
    root := &TreeNode{Val: preorder[0]}
    stack := []*TreeNode{root}
    for i := 1; i < len(preorder); i++ {
        lens := len(stack)
        node := stack[lens-1]
        if inorder[index] != node.Val {
            node.Left = &TreeNode{Val: preorder[i]}
            stack = append(stack, node.Left)
            continue
        }
        for lens > 0 && inorder[index] == stack[lens-1].Val {
            node = stack[lens-1]
            stack = stack[:lens-1]
            lens = len(stack)
            index++
        }
        node.Right = &TreeNode{Val: preorder[i]}
        stack = append(stack, node.Right)
    }
    return root
}
```

