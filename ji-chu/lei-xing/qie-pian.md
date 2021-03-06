# 切片

切片可以理解为数组的一段数组元素区间，其被设计用于弥补数组的两点不足：

1. 数组长度固定，不支持元素增删
2. 数组进行参数传递时采用值传递需要进行完整复制

## 定义切片

切片的定义方式与数组类似，只是不需要指定数组大小，在不对切片进行赋值的情况下默认值为`nil`，表示切片是空的

例子：定义切片

```go
var s1 []string
var s2 []int
```

## 切片创建

### 直接创建

可以通过两种方式创建切片，一种是通过内置的`make()`方法创建空切片

例子：创建空切片

```go
var s1 []string
// 通过内置方法make()创建切片，第一个参数表示切片类型，第二个参数表示切片中的元素长度，第三个参数表示切片可分配的空间大小
s1 = make([]string,5,10)

var s2 []int
// make方法可以省略第三个参数，在不指定的情况下切片的可分配总空间大小等于初始大小
s2 = make([]int,3)
```

可通过下图理解以上两个切片，灰色部分表示已经使用的空间，白色部分表示剩余可分配的空间

![&#x5207;&#x7247;s1&#x5185;&#x90E8;&#x7ED3;&#x6784;](../../.gitbook/assets/image%20%283%29.png)

![&#x5207;&#x7247;s2&#x5185;&#x90E8;&#x7ED3;&#x6784;](../../.gitbook/assets/image%20%282%29.png)

### 通过数组创建切片

另一种切片赋值方式是通过数组对切片进行赋值，赋值时需要指定数组的下标范围，格式为`[start:end]`，表示取数组下标`start`到下标`end-1`这个区间作为切片范围。

可以省略`start`和`end`,`[:end]`表示从下标0到下标`end-1`，`[start:]`表示从下标`start`到切片尾部下标，`[:]`表示从下标0到切片尾部下标

例子：通过数组创建切片

```go
package main

import (
	"fmt"
)

func main() {
	// 定义数组s1
	var s1 [5]string = [5]string{"mike", "lili", "mary", "tony", "lucy"}

	var s2 []string
	// 切片s2指向数组下标1到2
	s2 = s1[1:3]
	fmt.Println(s2)

	var s3 []string
	// 切片s3指向数组下标0到2
	s3 = s1[:3]
	fmt.Println(s3)

	var s4 []string
	// 切片s4指向数组下标2到4
	s4 = s1[2:]
	fmt.Println(s4)
}
```

以上代码的执行结果：

```text
[lili mary]
[mike lili mary]
[mary tony lucy]
```

## 切片元素读写

切片元素的读写方式与数组是一样的，都是通过下标修改元素值

例子：读取切片元素

```go
package main

import (
	"fmt"
)

func main() {
	// 定义切片s1
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}

	// 读取切片下标为3的元素
	fmt.Println(s1[3])

	// 修改切片下标为3的元素
	s1[3] = "mark"

	fmt.Println(s1[3])
}
```

以上代码的输出结果：

```text
tony
mark
```

## 切片元素追加

通过内置的`append()`方法为切片追加元素，第一个参数是要进行追加的切片，后续的参数是要追加到切片中的元素

切片中有两个属性：切片长度和存储空间大小

切片长度指的是切片中已赋值的元素数量，也同时作为切片元素追加时的开始位置，可通过内置方法`len()`获取切片长度

存储空间大小指的是切片所指向的数组大小，可通过内置方法`cap()`获取切片存储空间大小。当切片内容不断追加超过存储空间大小时，切片原先所指向的数组已经不足以存放切片元素，于是切片会重新创建并指向一个更大的新数组，把旧数组的内容复制到新数组中，在新数组中追加元素，新数组的大小等于旧数组的大小翻倍

{% hint style="info" %}
为了避免出现频繁的内存复制而导致的性能损耗，在创建切片时应当指定一个合适的可分配空间大小，在时间和空间上做取舍
{% endhint %}

例子：追加切片元素

```go
package main

import (
	"fmt"
)

func main() {
	var s1 []string
	// 创建切片s1
	s1 = make([]string, 6, 7)

	// 第一次追加，s1的长度从6变为7
	s1 = append(s1, "append1")
	fmt.Println(s1)
	fmt.Println(len(s1))
	fmt.Println(cap(s1))

	// 第二次追加，发现切片空间不够，于是重新创建一个更大的存储空间存放切片内容，存储空间大小为原先的大小7加上可分配空间大小7一共14，长度从7变为8
	s1 = append(s1, "append2")
	fmt.Println(s1)
	fmt.Println(len(s1))
	fmt.Println(cap(s1))
}
```

以上代码的运行结果：

```text
[      append1]
7
7
[      append1 append2]
8
14
```

`append()`方法支持不定参数，可同时合并多个元素

例子：追加多个切片元素

```go
package main

import (
	"fmt"
)

func main() {
	// 定义切片s1
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}

	fmt.Println(s1)

	// 追加多个元素
	s1 = append(s1, "egg", "milk")

	fmt.Println(s1)
}
```

以上代码的执行结果：

```text
[mike lili mary tony lucy]
[mike lili mary tony lucy egg milk]
```

## 切片元素合并

可以通过关键字`...`，将切片拆分为多个独立元素，将拆分后的元素传入`append()`方法即可完成切片合并

例子：切片合并

```go
package main

import (
	"fmt"
)

func main() {
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}

	var s2 []string = []string{"milk", "checken", "egg"}
	
	// 将切片s2中的元素拆分追加到切片s1中
	s1 = append(s1, s2...)

	fmt.Println(s1)
}
```

以上代码的执行结果：

```text
[mike lili mary tony lucy milk checken egg]
```

## 切片元素插入

插入元素到切片中的实现方式是将插入位置后半部的切片先临时保存，再为前半部的切片追加要插入的元素，最后追加临时保存的后半部切片

例子：切片插入

```text
package main

import (
	"fmt"
)

func main() {
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}
	var v1 = "trump"
	
	// 在切片s1的下标位置n插入元素v1
	n := 3
	tmp := append([]string{}, s1[n:]...)
	s1 = append(s1[:n], v1)
	s1 = append(s1, tmp...)

	fmt.Println(s1)
}
```

以上代码的执行结果：

```text
[mike lili mary trump tony lucy]
```

## 切片元素删除

切片本身并不能直接删除元素，而是通过`append()`方法对切片进行重新赋值，其原理是将要删除的元素之后的部分合并到要删除的元素之前的部分，以此覆盖掉要删除的元素。

例子：切片元素删除

```go
package main

import (
	"fmt"
)

func main() {
	var n = 2
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}

	// 将下标n之后的元素追加到下标n之前的元素中，覆盖下标为n的元素
	s1 = append(s1[0:n], s1[n+1:]...)

	fmt.Println(s1)
}
```

以上代码的执行结果：

```text
[mike lili tony lucy]
```

## 切片元素复制

可通过内置方法`copy()`，实现切片复制。在复制切片时，如果目标切片长度小于源切片长度，源切片中多出的部分会被忽略掉；如果目标切片长度大于源切片长度，目标切片多出的部分内容会保持不变。

例子：切片元素复制

```go
package main

import (
	"fmt"
)

func main() {
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}
	fmt.Println(s1)

	// 目标切片长度与源切片长度一致
	var s2 []string = make([]string, 5)
	copy(s2, s1)
	fmt.Println(s2)

	// 目标切片长度大于源切片长度
	var s3 []string = make([]string, 7)
	s3[5] = "maru"
	s3[6] = "bruce"
	copy(s3, s1)
	fmt.Println(s3)

	// 目标切片长度小于源切片长度
	var s4 []string = make([]string, 3)
	copy(s4, s1)
	fmt.Println(s4)
}
```

以上代码的执行结果：

```text
[mike lili mary tony lucy]
[mike lili mary tony lucy]
[mike lili mary tony lucy maru bruce]
[mike lili mary]
```

## 切片引用传递

切片中内置了指向数组的指针，对于切片的元素的读写是通过指针寻址到对应的源数组元素中进行读写的。在通过方法传参数时，虽然会重新在方法体中复制一个新的切片，但是新切片中的指针值是不变的，还是指向了原来的数组，因此在方法体内去修改切片的值，方法体外的切片值也会随着修改，两者指向的是同一个内存空间的数组。

例子：切片引用传递

```go
package main

import (
	"fmt"
)

func main() {
	var s1 []string = []string{"mike", "lili", "mary", "tony", "lucy"}
	fmt.Println(s1)

	// 修改切片s1中下标为2的值为"mark"
	ChangeSlice(s1, 2, "mark")
	fmt.Println(s1)
}

// 定义一个方法，该方法的作用是为切片slice中下标为index的元素赋值value
func ChangeSlice(slice []string, index int, value string) {
	slice[index] = value
}
```

以上代码的执行结果：

```text
[mike lili mary tony lucy]
[mike lili mark tony lucy]
```

