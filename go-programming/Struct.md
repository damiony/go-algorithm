# `Struct`

结构体是一种聚合的数据类型，由零个或多个任意类型的值聚合成的实体。

```go
type Student struct {
	Name   string
	Age    int
	Scores int
}
```

结构体变量的成员可以通过点操作符访问，点操作符也可以和指向结构体的指针一起工作。

```go
var stu1 Studnt
s.Name = "tom"

var stu2 *Student = &stu1
stu2.Age = 18
```

通常一行对应一个结构体成员，成员名字在前，类型在后。不过如果相邻成员类型相同，就可以合并到一行。

```go
type Student struct {
	Name        string
	Age, Scores int
}
```

如果成员顺序不同，那么就是不同类型的结构体。

一个命名为`S`的结构体类型，将不能再包含`S`类型的成员，因为一个聚合的值不能包含它自身。

`S`类型的结构体可以包含`*S`指针类型的成员。



## 零值

结构体类型的零值，是每个成员都是零值。



## 创建

直接创建：

```go
type Person struct {
    Name string
    Age int
}

var p Person
```

创建并且返回地址：

```go
pp := &Person{"jerry", 20}
// 等价于
// pp := new(Perosn)
// *pp = Person{"jerry", 20}
```



字面值顺序创建，一般只在定义结构体的包内部使用，或者是在较小的结构体中使用。

```go
type Person struct {
    Name string
    Age int
}

stu := PerSon{"tom", 20}
```



使用成员名和值来初始化：

```go
type Person struct {
    Name string
    Age int
}

stu := Person{
    Name: "jerry",
    Age: 20,
}
```



## 比较

如果结构体成员都可以比较，那么结构体也可以比较。



## 嵌入

结构体中可以包含另一个结构体类型的成员。

```go
type Point struct {
    X, Y int
}

type Circle struct {
    Center Point
    Radius int
}

type Wheel struct {
    Circle Circle
    Spokes int
}
```

可以通过点操作符访问结构体成员嵌套的成员。

```go
var w Wheel
w.Circle.Center.X = 8
```



## 匿名成员

可以在结构体中只声明一个成员的类型，而不指定成员的名字。

匿名成员的数据类型，必须是命名类型，或指向一个命名类型的指针。

```go
type Circle struct {
	Point
    Radius int
}

type Wheel struct {
    Circle
    Spokes int
}
```

可以直接访问呢叶子属性，不需要给出完整的路径：

```go
var w Wheel
w.X = 8
```

结构体字面值并没有简短表示匿名成员的语法。



匿名成员的名字由类型隐式地决定，所以不能同时包含两个类型相同的匿名成员。

如果匿名成员不导出，可以用简短形式访问匿名成员嵌套的成员，但是不能用简短形式访问匿名成员本身。