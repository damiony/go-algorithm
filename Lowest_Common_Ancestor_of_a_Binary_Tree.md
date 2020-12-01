二叉树的最近公共祖先

----

### 题目描述

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

示例1：

```shell
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
```

示例2：

```shell
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
```

----

### 解法

解法一：递归后序遍历

- 时间复杂度: `O(N)`
- 空间复杂度: `O(N)`

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    if root == nil {
        return root
    }
    if root.Val == p.Val || root.Val == q.Val {
        return root
    }
    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)
    if left != nil && right != nil {
        return root
    }
    if left == nil {
        return right
    }
    return left
}
```



解法二：哈希法

- 时间复杂度：`O(N)`
- 空间复杂度：`O(N)`

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    ancestor := map[int]*TreeNode{}
    history := map[int]*TreeNode{}
    queue := []*TreeNode{root}
    for len(queue) > 0 {
        node := queue[0]
        queue = queue[1:]
        if node == nil {
            continue
        }
        if node.Left != nil {
            ancestor[node.Left.Val] = node
            queue = append(queue, node.Left)
        }
        if node.Right != nil {
            ancestor[node.Right.Val] = node
            queue = append(queue, node.Right)
        }
    }
    for p != nil {
        history[p.Val] = p
        p = ancestor[p.Val]
    }
    for q != nil {
        if history[q.Val] != nil {
            return history[q.Val]
        }
        q = ancestor[q.Val]
    }
    return nil
}
```

