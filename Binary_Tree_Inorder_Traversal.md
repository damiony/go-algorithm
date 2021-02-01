### 题目描述

**二叉树的中序遍历:**

给定一个二叉树，返回它的中序遍历

**示例：**

```shell
输入：[1, null, 2, 3]
输出：[1,3,2]
```

### 解法

`go`语言中二叉树的定义为：

```go
type TreeNode struct {
  Val int
  Left *TreeNode
  Right *TreeNode
}
```

**方法一：递归法**

这是最常规的解法，但是不推荐

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func inorderTraversal(root *TreeNode) []int {
  var result []int
 
  if root == nil {
    return result
  }
    
  result = append(result, inorderTraversal(root.Left)...)
  result = append(result, root.Val)
  result = append(result, inorderTraversal(root.Right)...)
  return result
}
```

**方法二：迭代法**

递归的本质是使用了系统栈，迭代法正是利用了这种机制，在代码中手动生成栈。

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func inorderTraversal(root *TreeNode) []int {
    var result []int
    if root == nil {
        return result
    }
    
    var stack []*TreeNode

    for len(stack) > 0 || root != nil {
        for root != nil {
            stack = append(stack, root)
            root = root.Left
        }

        index := len(stack) - 1
        node := stack[index]
        stack = stack[:index]

        result = append(result, node.Val)
        root = node.Right
    }
    return result
}
```

**方法三：morris遍历**

这种解法比较晦涩，但是多看几遍代码就能理解其思想

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func inorderTraversal(root *TreeNode) []int {
	var result []int
	
	for root != nil {
		if root.Left == nil {
			result = append(result, root.Val)
			root = root.Right
			continue
		}

		leftTree := root.Left
		for leftTree.Right != nil && leftTree.Right != root {
			leftTree = leftTree.Right
		}

        if leftTree.Right != nil {
            result = append(result, root.Val)
            root = root.Right
            leftTree.Right = nil
        } else {
	        leftTree.Right = root
		    root = root.Left
        }

	}

	return result
}
```