# 方法

方法是对一段代码逻辑的封装，关键字`func`，定义方法的格式：

```text
func 方法名(传入参数)返回参数 {
    方法体
}
```

例子：定义一个方法

```go
// 定义一个Add方法，传入整型参数x和y
func Add(x int, y int) z int {
    // 进行将x加y的结果赋值给返回参数z
    z = x + y
    // return跳出方法体
    return
}
```

当传入参数类型相同时，可以进行简写，上述例子中传入参数`x int, y int`可以简写为`x,y int`

也可以隐藏返回参数名，返回匿名参数。

例子：方法返回匿名参数

```go
// 定义一个Add方法，传入整型参数x和y
func Add(x , y int) int {
    // 返回x加y的结果
    return x+y
}
```

方法还支持多参数的返回，同时返回多个结果

例子：返回多个参数

```go
package main

import (
	"fmt"
)

// Exchange 方法用于将传入参数交换位置并返回，传入参数x和y
func Exchange(x, y int) (int, int) {
	// 将x和y交换位置返回
	return y, x
}

func main() {
	var a, b = 1, 2
	// 将a和b交换位置并返回，分别赋值给c和d
	c, d := Exchange(a, b)
	fmt.Println(c, d)
}
```

以上代码的执行结果：

```text
2 1
```

对于返回多个参数的方法，如果有些返回值不需要使用时可以用关键字`_`忽略，在上述的例子中

```go
c, d := Exchange(a, b)
```

如果不希望接收第二个参数，可以写为

```go
c, _ := Exchange(a, b)
```



