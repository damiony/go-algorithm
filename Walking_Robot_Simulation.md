模拟行走机器人

----

### 题目描述

机器人在一个无限大的网格上行走，从点`(0, 0)`出发，面向北方。

机器人接受三种类型指令：

- `-2`：向左转 90 度
- `-1`：向右转 90 度
- `1 <= x <= 9`：向前移动 `x` 个单位长度

在网格上，有一些格子会被视为障碍物。机器人遇到障碍物会停下来，等待接受下一个指令。

请返回从原点到机器人经过的路径点的最大距离平方。

示例1：

```bash
输入: commands = [4,-1,3], obstacles = []
输出: 25
解释: 机器人将会到达 (3, 4)
```

示例2：

```bash
输入: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
输出: 65
解释: 机器人在左转走到 (1, 8) 之前将被困在 (1, 4) 处
```

----

### 解法

解法一：这种解法具体归类为什么思想我也不清楚。

- 时间复杂度：`O(N + K)`，N、K分别代表`commands`和`obstacles`的长度
- 空间复杂度：`O(K)`

```go
func robotSim(commands []int, obstacles [][]int) int {
	obstacleM := map[int]struct{}{}
	for _, pos := range obstacles {
        tmp := pos[0] << 16 + pos[1]
		obstacleM[tmp] = struct{}{}
	}
	dx := []int{0, 1, 0, -1}
	dy := []int{1, 0, -1, 0}
	x, y, di := 0, 0, 0

	maxDis := 0
	for _, num := range commands {
		if num == -1 {
			di = (di + 1) % 4
		} else if num == -2 {
			di = (di + 3) % 4
		} else {
			for i := 0; i < num; i++ {
				nx := x + dx[di]
				ny := y + dy[di]
                tmp := nx << 16 + ny
				if _, ok := obstacleM[tmp]; ok {
					break
				}
				x, y = nx, ny
			}
			dis := x*x + y*y
			if dis > maxDis {
				maxDis = dis
			}
		}
	}
	return maxDis
}
```

