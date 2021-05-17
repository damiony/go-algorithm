# Interface

- 接口是一个或多个方法签名的集合，任何类型的方法集中只要拥有与之对应的全部方法，就表示它“实现”了该接口。

- 接口只有方法签名，没有实现。

- 接口没有数据字段。

- 可在接口中嵌入其他接口。

- 类型可实现多个接口。

- 任何类型都实现了空接口。

- 匿名接口可以用作变量类型，或结构体成员。

  ```go
  type Tester struct {
  	s interface {
  		String() string
  	}
  }
  
  type User struct {
  	id   int
  	name string
  }
  
  func (u *User) String() string {
  	return fmt.Sprintf("User %d, %s", u.id, u.name)
  }
  
  func main() {
  	tt := Tester{&User{1, "Tom"}}
  	fmt.Println(tt.s.String())
  }
  ```

  

## 类型断言

利用类型断言，可以判断接口是否为某个具体的接口。

```go
type User struct {
	id   int
	name string
}

func (u *User) String() string {
	return fmt.Sprintf("%d, %s", u.id, u.name)
}

func main() {
	var o interface{} = &User{1, "Tom"}
	if i, ok := o.(fmt.Stringer); ok {
    	fmt.Println(i)
	}
}
```



## 类型分支

可以使用`switch`做批量判断，不支持`fallthrough`。

```go
var o interface{} = &User{1, "Tom"}
switch v := o.(type) {
case nil:
	fmt.Println("nil")
case fmt.Stringer:
	fmt.Println(v)
case func() string:
	fmt.Println(v())
default:
	fmt.Println("unknown")
}
```

