# 变量

## 变量声明

golang中引入了关键字`var`用于声明变量，格式为：

```text
var {变量名称} {变量类型}
```

例子： 声明变量

```go
// 声明一个整型变量，默认值0
var v1 int
// 声明一个浮点型变量，默认值0
var v2 float32
// 声明一个数组变量，默认值nil
var v3 [10]int 
// 声明一个切片变量，默认值nil
var v4 []float32
// 声明一个结构体变量，默认值{age:0}
var v5 struct {
    age int
}
// 声明一个指针变量，默认值nil
var v6 *int
// 声明一个字典变量，默认值nil
var v7 map[string]string
// 声明一个方法变量，默认值nil
var v8 func(x int)int
// 声明一个接口变量，默认值nil
var v9 interface{}
```

## 变量赋值

在声明变量时，可以为变量进行初始化赋值，有以下方式：

指定变量类型

```text
var {变量名} {变量类型} = {变量值}
```

还可以进行匿名赋值，不指定变量类型，根据变量值的类型自动推断出变量类型

```text
var {变量名} = {变量值}
```

或

```text
{变量名} := {变量值}
```

例子：声明变量并初始化赋值

```go
var v1 int = 30
var v2 = 50
v3 := 79
```



