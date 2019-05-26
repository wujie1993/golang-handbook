# 指针

指针是对于变量的间接引用，其中存放了变量的内存地址。指针通过寻址可对原有的变量进行访问和修改，主要应用于以下几个场景：

1. 作为方法传参参数，指向占内存空间大的变量，以节约方法传参过程中的内存开销；
2. 作为方法传参参数，指向需要在方法体内发生值变动后，在方法体外的值也同时需要发生变动的变量；
3. 定义结构体指针方法，通过引用传递实现在方法体中修改源结构体中的字段值；
4. 用于反射，获取所指向的元素类型。

与指针相关的关键字有两个`*`和`&`

关键字`*`用于定义指针和获取指针的值

关键字`&`，用于获取元素的指针地址

## 指针定义

定义指针的格式：

```text
var {指针名称} * {指针所指向的类型}
```

指针在未赋值的情况下值为`nil`

例子：定义指针

```go
// 定义一个指向整型的指针
var p1 *int

// 定义一个指向浮点型的指针
var p2 *float32
```

## 指针赋值

可通过`&`获取元素地址并赋值给指针，也可以通过内置的`new()`创建一个变量并把地址值赋值给指针

例子：指针赋值

```go
// 定义整型变量
var v1 int
// 定义一个指向整型的指针
var p1 *int 
// 使用&获取整型变量地址并赋值给指针
p1 = &v1

// 定义一个指向浮点型的指针
var p2 *float32 = new(float32)
```

## 指针引用传递

通常情况下，方法体内是无法修改方法体外的值的，因为方法传参的时候是进行值传递，每次传递的参数都是在方法体内新建的元素，与方法体外的元素只是值相同，并不存在直接关系。

由于指针在方法体传递的过程中传递的是一个地址，方法体内是可以通过这个地址寻址到原来方法体外的参数的，于是就可以通过指针做到在方法体内修改方法体外的值，可以称为指针的引用传递

例子：指针的引用传递

```go
package main

import (
	"fmt"
)

func main() {
	var v1 int
	// 定义一个指向整型的指针
	var p1 *int = &v1

	fmt.Println(*p1)

	// 通过指针传参，在方法体内修改方法体外的值
	FuncA(p1)

	fmt.Println(*p1)
}

// 定义一个使用指针参数的方法
func FuncA(p *int) {
	// 通过*寻址到指针所指向的元素，修改元素的值
	*p = 1
}
```

{% embed url="https://play.golang.org/p/X9UmzOPjw-L" caption="在线例子：指针的引用传递" %}

以上代码的执行结果：

```text
0
1
```

## 指针反射

例子：通过指针反射获取元素类型

```go
package main

import (
	"fmt"
	// 引用反射包
	"reflect"
)

type Person struct {
	Name string
	Age  int8
}

func main() {
	// 创建Person的实例
	p1 := &Person{}

	// 获取结构体实例的反射类型对象
	typeOfPerson := reflect.TypeOf(p1)

	// 取类型的元素
	typeOfPerson = typeOfPerson.Elem()

	// 显示反射类型对象的名称和种类
	fmt.Printf("element name: '%v', element kind: '%v'\n", typeOfPerson.Name(), typeOfPerson.Kind())
}
```

{% embed url="https://play.golang.org/p/m3X5gl5iScS" caption="在线例子：通过指针反射获取元素类型" %}

以上代码的执行结果：

```text
element name: 'Person', element kind: 'struct'
```



