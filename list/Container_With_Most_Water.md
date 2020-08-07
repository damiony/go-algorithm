盛最多水的容器

----

### 题目描述

给你`n`个非负整数`a1，a2，...，an`每个数代表坐标中的一个点`(i, ai)`。在坐标内画`n`条垂直线，垂直线`i`的两个端点分别为`(i, ai)`和`(i, 0)`。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

----

### 说明

如果输入数组为：`[1,8,6,2,5,4,8,3,7]`，容器能够容纳水的最大值为`49`。

----

### 解法

双指针法：

- 时间复杂度：O(N)
- 空间复杂度：O(1)

golang：

```go
func maxArea(height []int) int {
    var max, area int

    i := 0
    j := len(height) - 1
    for i < j {
        if height[i] >= height[j] {
            area = height[j] * (j - i)
            j--
        } else {
            area = height[i] * (j - i)
            i++
        }
        if max < area {
            max = area
        }
    }

    return max
}
```

javascript：

```javascript
var maxArea = function(height) {
    let i = 0;
    let j = height.length - 1;
    let max = 0;
    let area = 0;

    while (i < j) {
        if (height[i] < height[j]) {
            area = height[i] * (j - i);
            i++;
        } else {
            area = height[j] * (j - i);
            j--;
        }
        if (max < area) {
            max = area;
        }
    }

    return max
};
```