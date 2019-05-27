# 结构体

结构体是一种自定义类型，是实现面向对象编程的基础，我们可以通过结构体去描述一个事物所具有的各项特性，形成对于对象的描述。如定义一个人的类型，并指定其所具有的一些特征，诸如姓名，性别，年龄，身高等，也能为我们所定义的这个人类型赋予各种功能方法，如自我介绍，开车，跳舞等。

## 结构体定义

在结构体中可以自定义各种类型的字段，类似于树形的结构，用于存放结构复杂的数据，关键字`struct`

结构体的定义格式如下：

```text
type {结构体名称} struct{
    {字段名} {字段类型}
}
```

例子：定义结构体类型

```go
// 定义一个名为Person的结构体
type Person struct{
    // 定义字符串类型字段Name，
    Name string
    // 定义整型字段Age
    Age int8
}
```

## 结构体创建

有了结构体类型后，就可以用于创建结构体，在结构体创建时可以进行初始化赋值，也可以通过`变量名.字段名`的方式去读取和修改字段值

例子：创建结构体

```go
package main

import (
	"fmt"
)

// 定义一个名为Person的结构体
type Person struct {
	// 定义字符串类型字段Name，
	Name string
	// 定义整型字段Age
	Age int8
	// 定义浮点型字段Height
	Height float32
}

func main() {
	// 定义一个Person类型的结构体变量,并初始化其中的字段值
	var me Person = Person{
		Name: "wj",
		Age:  26,
	}
	// 修改结构体变量中的字段值
	me.Height = 1.78

	// 获取并输出结构体变量中的字段值
	fmt.Println(me.Name)
	fmt.Println(me.Age)
	fmt.Println(me.Height)
}
```

{% embed url="https://play.golang.org/p/CDv\_gN3tpBp" caption="在线例子：定义结构体" %}

以上代码的执行结果：

```text
wj
26
1.78
```

## 字段可访问性

与包类似，结构体中的定义的字段是具有可访问性的。以小写开头的字段或方法在结构体外部是无法访问到的，相当于私有字段。代码编写时应当遵守最小权限原则，即只有确定是要被外部访问的字段，才会将其设置为大写开头的公开字段，这是为了保证代码的安全性。

例子：定义具有私有字段的结构体类型

```go
// 定义一个名为Person的结构体
type Person struct{
    // 定义字符串类型字段Name，
    Name string
    // 定义整型字段Age
    Age int8
    // 定义浮点型私有字段weight
    weight float32
}
```

## 结构体方法

在结构体类型中可以定义各种方法，称为类型方法。区别是否该把方法定义为类型方法还是普通方法的依据是，该方法是否在逻辑上是每个类型实体都具有的一个功能，并且与当前类型实体中的属性都是有关联的。

例子：结构体方法

```go
package main

import (
	"fmt"
)

// 定义一个名为Person的结构体
type Person struct {
	// 定义字符串类型字段Name，
	Name string
	// 定义整型字段Age
	Age int8
	// 定义私有浮点型字段weight
	weight float32
}

func (p Person) Introduce() {
	// 通过结构体方法访问私有字段
	fmt.Printf("My name is %s,weight %f", p.Name, p.weight)
}

func main() {
	// 定义一个Person类型的结构体变量,并初始化其中的字段值
	var p1 Person = Person{
		Name:   "tony",
		Age:    32,
		weight: 147.8,
	}

	// 调用结构体方法
	p1.Introduce()
}
```

{% embed url="https://play.golang.org/p/0LRzWWzTP0D" caption="在线例子：结构体方法" %}

以上代码的执行结果：

```text
My name is tony,weight 147.800003
```

结构体方法在执行的时候，会在内存中创建一个新的结构体并执行其中的方法，也就是说，在结构体方法中修改原结构体的内容是不生效的。我们可以借助于指针的引用传递效果，定义一个结构体指针方法，在该指针上调用方法时，可以借助于指针寻址到源结构体，从而修改源结构体中的值，达到在结构体方法中修改结构体值的效果

例子：结构体指针方法

```go
package main

import (
	"fmt"
)

// 定义一个名为Person的结构体
type Person struct {
	// 定义字符串类型字段Name，
	Name string
	// 定义整型字段Age
	Age int8
	// 定义私有浮点型字段weight
	weight float32
}

func (p *Person) ChangeName(newName string) {
	p.Name = newName
}

func main() {
	// 定义一个Person类型的结构体指针,并初始化其中的字段值
	var p1 *Person = &Person{
		Name:   "tony",
		Age:    32,
		weight: 147.8,
	}

	fmt.Println(p1.Name)

	// 通过结构体指针方法修改结构体中的值
	p1.ChangeName("mike")

	fmt.Println(p1.Name)
}
```

{% embed url="https://play.golang.org/p/gEgUAh0HV8p" caption="在线例子：结构体指针方法" %}

以上代码的执行结果：

```text
tony
mike
```

{% hint style="info" %}
结构体指针方法只能通过结构体指针调用
{% endhint %}

结构体指针也可以调用结构体方法，在调用时，首先会把结构体方法转换成结构体指针方法，再对该转换后的结构体指针方法进行调用

