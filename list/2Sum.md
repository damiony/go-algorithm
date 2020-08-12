两数之和

----

### 题目描述

> 给定一个数组`nums`和一个目标值`target`，找出和为目标值`target`的两个整数，返回他们的数组下标。
>
> 假设每种输入对应一个答案，同一个元素不能使用两遍。

示例：

```shell
输入：nums=[2,7,11,15] target=9
输出：[0,1]
```

----

### 解法

方法一：暴力求解

对于每一个元素，遍历数组找出满足要求的答案，然后返回

- 时间复杂度: O(n^2)
- 空间复杂度: O(1)



方法二：双指针法 + 两遍遍历

将元素及其下标，存储在哈希表中。遍历数组元素时，可直接在哈希表查找是否存在满足条件的数据，省去了再次遍历数组的开销

- 时间复杂度：O(n)
- 空间复杂度：O(n)



方法三：双指针法 + 一次遍历

遍历数组时，在哈希表中查找是否存在满足条件的数据，如果不存在，则将当前元素存入哈希表，然后继续遍历。

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func twoSum(nums []int, target int) []int {
    result := []int{}
    numMap := make(map[int]int)

    for i := 0; i < len(nums); i++ {
        sub := target - nums[i]
        if index, ok := numMap[sub]; ok {
            result = append(result, i, index)
            break
        }
        numMap[nums[i]] = i
    }

    return result
}
```