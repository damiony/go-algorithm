# Map

> 哈希表是无序的`key/value`对集合，检索、更新、删除的时间复杂度为`O(1)`



## 类型

`map`类型的写法为：`map[key]value`。

`key`的类型相同，`value`的类型也相同。

`key`必须是支持 `==` 比较运算符的数据类型，用浮点类型做`key`是坏的想法。



## 语法

- 创建

  可以直接创建`nil`值的`map`:
  
  ```go
  var ages map[string]int
  ```
  
  可以使用`make`创建`map`:
  
  ```go
  ages := make(map[string]int)
  ```
  
  还可以使用字面值语法创建`map`:
  
  ```go
  ages := map[string]int{
      "alice": 31,
      "charlie": 34,
  }
  // 这相当于
  // ages := make(map[string]int)
  // ages["alice"] = 31
  // ages["charlie"] = 34
  ```

- 访问

  可以使用下标访问：
  
  ```go
  ages["alice"] = 32
  ```
  
  如果查找失败，将返回`value`类型的零值。
  
  简短复制语法可以用在`map`上：
  
  ```go
  ages["bob"] += 1
  // ages["bob"]++
  ```
  
  禁止对`map`中的元素进行取地址操作，`map`可能随着元素数量的增长重新分配内存空间，从而可能导致之前的地址无效。
  
- 删除

  可以使用`delete`函数删除

  ```go
  delete(ages, "alice")
  ```

- 遍历

  可以使用`range`风格的`for`循环实现：

  ```go
  for name, age := range ages {
      fmt.Println(name, age)
  }
  ```

  遍历的顺序是随机的，如果想顺序遍历`key/value`对，必须显示对`key`进行排序，可以使用`sort`包的`Strings`函数对字符串`slice`进行排序。

  ```go
  names := []string{}
  for name := range ages {
  	names = append(names, name)
  }
  sort.Strings(names)
  for _, name := range names {
  	fmt.Println(ages[name])
  }
  ```

- 零值

  `map`类型的零值是`nil`。

  查找、删除、`len`、`range`都可以安全工作在`nil`值的`map`上，但是向一个`nil`值的`map`存入元素将导致一个`panic`异常。

- 检查元素是否存在

  ```go
  if age, ok := ages["bob"]; !ok { /*...*/ }
  ```

- 比较

  只可以与`nil`进行比较操作。

  如果要判断两个`map`是否相同：

  ```go
  func equal(x, y map[string]int) bool {
  	if len(x) != len(y) {
  		return false
  	}
  
  	for k, xv := range x {
  		if yv, ok := y[k]; !ok || yv != xv {
  			return false
  		}
  	}
  	return true
  }
  ```

  
