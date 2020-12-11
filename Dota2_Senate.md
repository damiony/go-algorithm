Dota2 参议院

----

### 题目描述

ota2 的世界里有两个阵营：Radiant(天辉)和 Dire(夜魇)

Dota2 参议院由来自两派的参议员组成。现在参议院希望对一个 Dota2 游戏里的改变作出决定。他们不停的进行轮回循环投票。在每一轮中，每一位参议员都可以行使两项权利中的一项：

- 禁止一名参议员的权利：

  参议员可以让另一位参议员在这一轮和随后的几轮中丧失所有的权利。

- 宣布胜利：如果参议员发现有权利投票的参议员都是同一个阵营的，他可以宣布胜利并决定在游戏中的有关变化。

 


给定一个字符串代表每个参议员的阵营。字母 “R” 和 “D” 分别代表了 Radiant（天辉）和 Dire（夜魇）。

假设每一位参议员都足够聪明，会为自己的政党做出最好的策略，你需要预测哪一方最终会宣布胜利并在 Dota2 游戏中决定改变。输出应该是 Radiant 或 Dire。

示例：

```bash
输入："RD"
输出："Radiant"
```

----

### 解法

解法一：队列

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go

func predictPartyVictory(senate string) string {
    qr := []int{}
    qd := []int{}
    n := len(senate)
    for i := 0; i < n; i++ {
        if senate[i] == 'R' {
            qr = append(qr, i)
        }
        if senate[i] == 'D' {
            qd = append(qd, i)
        }
    }

    for len(qd) > 0 && len(qr) > 0 {
        r1, d1 := qr[0], qd[0]
        qr, qd = qr[1:], qd[1:]
        if r1 < d1 {
            qr = append(qr, r1 + n)
        } else {
            qd = append(qd, d1 + n)
        }
    }
    if len(qd) > 0 {
        return "Dire"
    }
    return "Radiant"
}
```

解法二：循环

- 时间复杂度：`O(n)`
- 空间复杂度：`O(n)`

```go
func predictPartyVictory(senate string) string {
    flag := make([]bool, len(senate))
    rBans, dBans := 0, 0
    for {
        rCnt, dCnt := 0, 0
        for i := 0; i < len(senate); i++ {
            if flag[i] {
                continue
            }
            if senate[i] == 'R' {
                if rBans > 0 {
                    rBans--
                    flag[i] = true
                } else {
                    rCnt++
                    dBans++
                }
            }
            if senate[i] == 'D' {
                if dBans > 0 {
                    dBans--
                    flag[i] = true
                } else {
                    dCnt++
                    rBans++
                }
            }
        }
        if rCnt == 0 {
            return "Dire"
        }
        if dCnt == 0 {
            return "Radiant"
        }
    }
}
```

