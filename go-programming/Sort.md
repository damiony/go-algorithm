# Sort排序

可以使用`sort`包对数据集合进行排序，该包实现了四种基本排序算法：插入排序、归并排序、堆排序和快速排序。



## 数据集合排序

- 数据集合需要实现`sort.Interface`接口的三个方法，该接口定义为：

  ```go
  type Interface interface {
      Len() int
      Less(i, j int) bool
      Swap(i, j int)
  }
  ```

- 数据集合实现了这三个方法后，即可调用`sort`包的`Sort()`方法排序。`Sort()`方法定义为：

  ```go
func Sort(data interface)
  ```

- 判断数据集合是否已经排好序，可以使用`IsSorted`方法：

  ```go
  func IsSorted(data Interface) bool {
      n := data.Len()
      for i := n - 1; i > 0; i-- {
          if data.Less(i, i-1) {
              return false
          }
      }
      return true
  }
  ```

- `Reverse()`允许将数据按`Less()`定义的排序方式逆序排序：

  ```go
  type reverse struct {
      Interface
  }
  
  func (r reverse) Less (i, j int) bool {
      return r.Interface.Less(j, i)
  }
  
  func Reverse(data Interface) Interface {
      return &reverse{data}
  }
  ```

- `Search()`方法使用二分查找算法来找到能使`f(x)(0<=x<n)`返回`true`的最小值`i`。

  前提条件条件：`f(x)(0<=x<i)`均返回`false`，`f(x)(i<=x<n)`均返回`true`，



## 示例

```go
type Person struct {
	age  int
	name string
}

type MyPerson []Person

func (m MyPerson) Len() int {
	return len(m)
}

func (m MyPerson) Less(i, j int) bool {
	return m[i].age < m[j].age
}

func (m MyPerson) Swap(i, j int) {
	m[i], m[j] = m[j], m[i]
	return
}

func main() {
    stus := MyPerson{
        {30, "c"},
        {25, "b"},
        {20, "a"},
    }
    sort.Sort(stus)
    fmt.Println(stus)
}
```





## 内部数据类型排序

`sort`包原生支持`[]int`、`[]float64`、`[]string`三种内建数据类型切片的排序操作。

- `[]int`

  ```go
  a := []int{5, 4, 12, 23, 42}
  sort.Ints(a)
  ```

  

- `[]float64`

  ```go
  a := []float64{1.3, 1.2, 0.4, 2.3}
  sort.Float64s(a)
  ```

  

- `[]string`

  ```go
  a := []string{"a", "b", "ab", "ba"}
  sort.Strings(a)
  ```

  

