# 常量

常量指的是在编译阶段就已知且不可改变的值

## 定义常量

定义常量使用关键字`const`

例子：定义常量

```go
const PI float32 = 3.1415926

const (
    length = 20
    size = 31
)

const c1,c2 = 1 , true

const c3,c4 int = 2,3
```

## 预定义常量

go语言中预先定义了几个常量：`true`、`false`和`iota`

其中`true`和`false`用于`bool`类型的取值

`iota`是一个自增常量，其作用是在一个`const`定义域内，`iota`每出现一次，其自身的值会自增1，从0开始取值

例子：定义自增常量

```go
const (
    None = iota // 取值0
    Monday = iota // 取值1
    Tuesday = iota // 取值2
    Wednesday = iota // 取值3
    Thusday = iota // 取值4
    Friday = iota // 取值5
    Saturday = iota // 取值6
    Sunday = iota // 取值7
)

const (
    c1 = iota * 26 // 取值0
    c2 = iota * 26 // 取值26
    c3 = iota * 26 // 取值52
)
```

如果在`const`定义域内的每个`iota`常量表达式是一样的，可以进行简写

例子：定义简写自增常量

```go
const (
    None = iota // 取值0
    Monday // 取值1
    Tuesday // 取值2
    Wednesday // 取值3
    Thusday // 取值4
    Friday // 取值5
    Saturday // 取值6
    Sunday // 取值7
)

const (
    c1 = iota * 26 // 取值0
    c2 // 取值26
    c3 // 取值52
)
```

{% hint style="info" %}
golang中并不存在枚举类型，通常用常量表示
{% endhint %}

例子：使用常量起到枚举效果

```go
const (
    OrderDesc = "desc"
    OrderAsc = "asc"
)

const (
    HTTPStatusCodeSucceed = 200
    HTTPStatusCodeUnauthorized = 401
    HTTPStatusCodeNotFound = 404
)
```

