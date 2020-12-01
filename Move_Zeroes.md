移动零

----

### 题目描述

给定一个数组`nums`，编写一个函数将所有`0`移动到数组的末尾，同时保持非零元素的相对顺序。

实例：
```shell
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

----

### 解法

方法一：可以使用额外数组，存放非零元素

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func moveZeroes(nums []int)  {
    copy := make([]int, 0, len(nums))
    for _, v := range nums {
        if v != 0 {
            copy = append(copy, v)
        }
    }

    for i := 0; i < len(nums); i++ {
        if i < len(copy) {
            nums[i] = copy[i]
        } else {
            nums[i] = 0
        }
    }
    return
}
```



方法二：双指针法，使用两次遍历

- 时间复杂度：O(n)
- 空间复杂度：O(1)

```go
func moveZeroes(nums []int)  {
    first := 0
    for i := 0; i < len(nums); i++ {
        if nums[i] == 0 {
            continue
        }
        nums[first] = nums[i]
        first++
    }

    for i := first; i < len(nums); i++ {
        nums[i] = 0
    }
    return
}
```



方法三：双指针法，使用一次遍历

- 时间复杂度：O(n)
- 空间复杂度：O(n)

```go
func moveZeroes(nums []int)  {
    first := 0
    for i := 0; i < len(nums); i++ {
        if nums[i] == 0 {
            continue
        }
        if (i != first) {
            nums[first], nums[i] = nums[i], nums[first]
        }
        first++
    }
    return
}
```