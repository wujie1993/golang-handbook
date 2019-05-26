# 数组

数组表示的是同一种数据类型的集合。数组中的每一个元素称之为数组元素,每个数组元素都有对应的序号，从0开始。定义数组时需要指定数组长度，数组长度必须是常量

例子：定义一个数组

```go
var s1 [32]byte
var s2 [22]int
```

当数组内的数组元素也是数组类型时，该数组称为多维数组

例子：定义一个多维数组

```go
var s1 [23][34]bool
```

在定义数组时可以进行初始化赋值，默认会自动从下标0开始赋值，也可以为数组元素指定下标

例子：数组初始化赋值

```go
package main

import (
	"fmt"
)

func main() {
	// 不指定下标的数组初始化赋值
	var s1 [5]string = [5]string{"mike","mary","tony","lucy","gigi"}
	fmt.Println(s1)
	
	// 指定下标的数组初始化赋值
	var s2 [5]string = [5]string{0:"mike",3:"mary",2:"tony",1:"lucy",4:"gigi"}
	fmt.Println(s2)
}
```

{% embed url="https://play.golang.org/p/HV445-0L8wh" caption="在线例子：数组初始化赋值" %}

以上代码的执行结果：

```text
[mike mary tony lucy gigi]
[mike lucy tony mary gigi]
```

数组元素是可变的，可以通过下标去读取和修改数组元素

例子：读取数组元素

```go
var s1 [3]string = [3]string{"a","b","c"}
// 将数组s1中下标为1的数组元素值赋值给c1,即"b"
c1:=s1[1]
```

例子：修改数组元素

```go
var s1 [3]string = [3]string{"a","b","c"}
// 修改数组s1中下标为2的数组元素值为"d"，修改后的数组内容为{"a","b","d"}
s1[2] = "d"
```

可以通过内置方法`len()`获取到数组的定义长度

例子：获取数组长度

```go
var s1 [3]string = [3]string{"a","b","c"}
// 获取数组s1的定义长度，赋值给length
length:= len(s1)
```

{% hint style="info" %}
数组是值类型，作为方法参数传递时，会完整复制出一个新的数组
{% endhint %}

