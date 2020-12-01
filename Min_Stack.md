最小栈

----

### 题目描述

设计一个支持`push`、`pop`、`top`操作，并且能在常数时间内检索到最小元素的栈

- push(x)：将元素x压入栈中
- pop()：删除栈顶元素
- top()：获取栈顶元素
- getMin()：检索栈中最小元素

提示：`pop`、`top`、`getMin`总是在非空栈上调用

示例：

```shell
输入：["MinStack", "push", "push", "push", "getMin", "pop", "top", "getMin"]
	[[], [-2], [0], [-3], [], [], [], []]
输出：[null, null, null, null, -3, null, 0, -2]
```

----

### 解法

解法一：使用一个辅助栈，存放当前栈中的最小值

- 时间复杂度：O(1)
- 空间复杂度：O(n)

```go
type MinStack struct {
	stack    []int
	minStack []int
}

/** initialize your data structure here. */
func Constructor() MinStack {
	return MinStack{}
}

func (m *MinStack) Push(x int) {
	m.stack = append(m.stack, x)

	minLen := len(m.minStack)
	if minLen <= 0 || m.minStack[minLen-1] >= x {
		m.minStack = append(m.minStack, x)
	}
}

func (m *MinStack) Pop() {
	stackLen := len(m.stack)
	if stackLen <= 0 {
		return
	}

	minLen := len(m.minStack)
    if minLen > 0 {
	    min := m.minStack[minLen-1]
	    ele := m.stack[stackLen-1]
	    if min == ele {
	    	m.minStack = m.minStack[:minLen-1]
	    }
    }

	m.stack = m.stack[:stackLen-1]
}

func (m *MinStack) Top() int {
	stackLen := len(m.stack)
	if stackLen <= 0 {
		return 0
	}

	return m.stack[stackLen-1]
}

func (m *MinStack) GetMin() int {
	minLen := len(m.minStack)
	if minLen <= 0 {
		return math.MaxInt64
	}
	return m.minStack[minLen-1]
}
```

