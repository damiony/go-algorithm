# 类型

可将类型分为两大类：

* 命名类型：`bool`、`int`、`string`等。
* 非命名类型：`array`、`slice`、`map`等和具体元素类型、长度有关的类型，属于未命名类型。



具有相同声明的未命名类型被视为同一类型：

- 具有相同基类型的指针。
- 具有相同元素类型和长度的`array`。
- 具有相同元素类型的`slice`。
- 具有相同键值类型的`map`。
- 具有相同元素类型和传送方向的`channel`。
- 具有相同字段序列（字段名、类型、标签、顺序）的匿名`struct`。
- 签名相同（参数和返回值，不包括参数名称）的`function`。
- 方法集相同（方法名、方法签名相同，和次序无关）的`interface`。

