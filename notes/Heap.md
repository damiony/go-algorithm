### 堆

- 堆是一种完全二叉树
- 堆中每个节点的值都大于等于（或者小于等于）子树节点的值，等价说法为，每个节点值都大于等于（小于等于）其左右子节点的值。

一般使用数组存储堆，因为使用数组存储完全二叉树这种结构比较节省空间。

数组下标从`1`开始，节点`i`的左节点为`2i`，右节点为`2i+1`。

### 堆化

- 自上向下堆化：

  父子节点进行对比，如果不满足大小关系，交换位置，一直重复这个过程，直到父子节点满足大小关系。

- 自下向上堆化

  当前节点与父节点比较大小，如果不满足大小关系，交换位置，一直重复这个过程，直到父子节点满足大小关系。

### 删除

堆的删除操作为：

- 删除堆顶元素。
- 将堆的最后一个元素移到堆顶。
- 自上向下堆化

时间复杂度：`O(logn)`

### 插入

插入操作为：

- 在堆的最后位置插入数据
- 自下向上堆化

时间复杂度：`O(logn)`

### 堆排序

堆排序是一种原地、不稳定的排序算法。

- 建堆

  一般使用自上向下的堆化方法，从下标`n/2`开始，到下标`1`结束

  自上向下建堆的时间复杂度是`O(n)`

  自下向上建堆的时间复杂度为`O(nlogn)`

- 排序

  将堆顶元素移到最后，然后对剩下的`n-1`个元素进行堆化，一直重复这个过程，直到剩下的元素个数为`1`

  排序的时间复杂度是`O(nlogn)`

总体的时间复杂度是：`O(nlogn)`

### 堆排序 VS 快速排序

- 堆排序数据访问方式不友好
- 堆排序交换次数多于快速排序

### 堆排序代码

```go
package main

type heap struct {
	mem []int
	cnt int
}

func newHeap(m []int) *heap {
	h := &heap{mem: m, cnt: len(m)}
	// 自上向下堆化
	start := (h.cnt - 1 - 1) / 2
	for i := start; i >= 0; i-- {
		h.heapifyTop(i)
	}
	return h
}

// heapify 自下向上
func (h *heap) heapifyBottom() {
	i := h.cnt - 1
	for i >= 0 {
		if i/2 < 0 || h.mem[i/2] <= h.mem[i] {
			break
		}
		h.mem[i], h.mem[i/2] = h.mem[i/2], h.mem[i]
		i = i / 2
	}
	return
}

// heapify 自顶向下堆化
func (h *heap) heapifyTop(i int) {
	for i < h.cnt {
		max := i
		if 2*i+1 < h.cnt && h.mem[2*i+1] < h.mem[max] {
			max = 2*i + 1
		}
		if 2*i+2 < h.cnt && h.mem[2*i+2] < h.mem[max] {
			max = 2*i + 2
		}
		if max == i {
			break
		}
		h.mem[max], h.mem[i] = h.mem[i], h.mem[max]
		i = max
	}
	return
}

// Push 将元素放入堆中 如果堆满了 则比较堆顶
func (h *heap) Push(x int) {
	if len(h.mem) == h.cnt {
		if x > h.mem[0] {
			h.mem[0] = x
			h.heapifyTop(0)
		}
	} else {
		h.mem[h.cnt] = x
		h.cnt++
		h.heapifyBottom()
	}
	return
}

// Pop 弹出堆顶元素
func (h *heap) Pop() int {
	num := h.mem[0]
	h.mem[0], h.mem[h.cnt-1] = h.mem[h.cnt-1], h.mem[0]
	h.cnt--
	h.heapifyTop(0)
	return num
}

func (h *heap) Sort() []int {
	ans := make([]int, h.cnt)
	for i := range ans {
		ans[i] = h.Pop()
	}
	return ans
}
```