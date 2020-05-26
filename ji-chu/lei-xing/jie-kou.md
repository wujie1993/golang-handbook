# 接口

关键字`interface{}`

### 空接口

可以将所有的类型变量赋值给空接口，空接口可以进行类型断言，再转换为具体的类型变量

定义空接口的格式：

```text
var 变量名 interface{}
```

例子：空接口赋值与类型推断

```go
package main

import (
	"fmt"
)

func main() {
	// 定义并赋值一个变量v1
	var v1 int = 54

	// 将变量赋值给一个空接口i1
	var i1 interface{} = v1

	// 定义一个变量v2
	var v2 int
	// 对空接口i1进行类型推断，推断成功的话会把值赋给变量v2
	v2, _ = i1.(int)

	fmt.Println(v2)
}
```

{% embed url="https://play.golang.org/p/O72Pf1RuHE4" caption="在线例子：空接口赋值与类型推断" %}

以上代码的运行结果：

```text
54
```

### 类型接口

接口可用于定义一套标准的可操作行为规范，其中包含了各种方法的定义规范。与其他语言不同，golang的接口是一种非侵入式的接口，在类型定义的过程中中不需要知道接口的存在，只会在接口赋值时去判断类型与接口是否匹配。通过这种非侵入式的接口可以更进一步地对代码进行解耦。

定义接口的格式：

```text
type 类型名称 interface {
    方法名(传入参数)返回参数
    ...
}
```

例子：接口调用

```go
package main

import (
	"fmt"
)

// 定义一个Runner接口，其中包含Run()方法
type Runner interface {
	Run()
}

// 定义一个Swimmer接口，其中包含Swim()方法
type Swimmer interface {
	Swim()
}

// 定义一个Cat类型
type Cat struct{}

// Cat类型实现了Run方法，说明猫是Runner
func (c Cat) Run() {
	fmt.Println("cat can run.")
}

// 定义一个Dog类型
type Dog struct{}

// Dog类型实现了Run方法，说明狗是Runner
func (d Dog) Run() {
	fmt.Println("dog can run.")
}

// Dog类型实现了Swim方法，说明狗是Swimmer
func (d Dog) Swim() {
	fmt.Println("dog can swim.")
}

func main() {
	// 定义一个Runner接口变量runner，默认值nil
	var runner Runner
	// 定义一个Swimmer接口变量swimmer，默认值nil
	var swimmer Swimmer

	// 实例化一个Cat类型变量cat
	var cat Cat
	// 实例化一个Dog类型变量dog
	var dog Dog

	// 将cat赋值给接口runner
	runner = cat
	// 通过runner执行cat的Run()方法
	runner.Run()

	// 将dog赋值给接口runner
	runner = dog
	// 通过runner执行dog的Run()方法
	runner.Run()

	// 将dog赋值给接口swimmer
	swimmer = dog
	// 通过swimmer执行dog的Swim()方法
	swimmer.Swim()
}
```

{% embed url="https://play.golang.org/p/6GKV7l2LIfH" caption="在线例子：接口调用" %}

以上代码的执行结果：

```text
cat can run.
dog can run.
dog can swim.
```

可以这么理解，接口只关心类型能否去执行某个方法，而不会去关心类型中有什么值

### 类型转换

将接口类型变量转换成其它类型需要使用类型推断，有三种书写方式：

**方式一**

```go
{{ 推断后的变量 }} := {{ 接口变量 }}.({{ 推断类型 }})
```

{% hint style="info" %}
使用该方式需要确保推断类型是正确的，否则会引发panic错误
{% endhint %}

**方式二**

```go
{{ 推断后的变量 }}, {{ 是否推断成功 }} := {{ 接口变量 }}.({{ 推断类型 }})
```

**方式三**

```go
switch {{ 推断后的变量 }}:={{ 接口变量 }}.(type){
case {{ 推断类型1 }}:
    ...
case {{ 推断类型2 }}:
    ...
default:
    ...
}
```

对于参数类型不确定的情况，推荐使用方式三

例子：通过类型推断对接口变量进行格式转换

```go
package main

import "fmt"

func main() {
	var i interface{}
	
	// 类型推断方式一
	i = 666
	var v1 int
	v1 = i.(int)
	fmt.Printf("int:%d\n", v1)
	
	// 类型推断方式二
	i = "hello"
	var v2 string
	v2, ok := i.(string)
	if !ok{
		panic("not a string type")
	}
	fmt.Printf("string:%s\n", v2)
	
	// 类型推断方式三
	i = 3.1415926
	var v3 float64
	switch v:=i.(type){
	case float64:
		v3 = v
	default:
		panic("not type of float64")
	}	
	fmt.Printf("float:%f\n", v3)
}

```

