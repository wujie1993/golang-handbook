# 循环

`for`循环语句中包含3个部分，初始化语句，条件判断语句和循环标记语句

初始化语句在进入`for`循环体前执行，用于初始化循环用的变量，只执行一次

条件判断语句在每轮循环开始前执行，当条件成立时才会继续执行循环体中的内容，否则跳出循环

循环结尾语句在每轮循环的结尾执行，主要用于为条件判断语句中的变量进行赋值

语法格式：

```text
for 初始化语句;条件判断语句;循环结尾语句 {
    ...
}
```

例子：使用for循环遍历切片

```go
package main

import (
	"fmt"
)

func main() {
	// 初始化切片
	s1 := []string{"mike", "lili", "mary", "tony", "lucy"}
	// 获取切片长度
	length := len(s1)

	// 首先对i进行初始化赋值，当i小于切片长度时，执行循环体中的内容，每轮循环结束后i自增1
	for i := 0; i < length; i++ {
		fmt.Println(s1[i])
	}
}
```

{% embed url="https://play.golang.org/p/J6RhK8J4Agu" caption="在线例子：使用for循环遍历切片" %}

以上代码的执行结果：

```text
mike
lili
mary
tony
lucy
```

`for`循环也可以与关键字`range`结合，用于对数组、切片、字典和通道的遍历，语法格式：

```text
for 下标,元素值 := range 可遍历的变量 {
    ...
}
```

对数组、切片和字典进行遍历时，可以省略元素值：

```text
for 下标 := range 数组、切片或字典 {
    ...
}
```

对通道进行遍历时，省略下标：

```text
for 元素值 := range 通道 {
    ...
}
```

例子：通过range遍历切片

```go
package main

import (
	"fmt"
)

func main() {
	// 初始化切片
	s1 := []string{"mike", "lili", "mary", "tony", "lucy"}

	// 遍历切片，下标为index，值为value。value是复制出来的值，与原先切片中的元素不是同一个
	for index, value := range s1 {
		// 通过切片下标输出切片元素
		fmt.Println(s1[index])
		// 通过遍历获取到的元素值输出
		fmt.Println(value)
	}
}
```

{% embed url="https://play.golang.org/p/ecHgaJyYO7A" caption="在线例子：通过range遍历切片" %}

以上代码的执行结果：

```text
mike
mike
lili
lili
mary
mary
tony
tony
lucy
lucy
```

例子：通过range遍历字典

```go
package main

import (
	"fmt"
)

func main() {
	// 定义字典
	var m1 map[string]string
	// 初始化字典
	m1 = make(map[string]string)

	// 为字典赋值
	m1["key1"] = "value1"
	m1["key2"] = "value2"

	// 遍历字典
	for key, value := range m1 {
		fmt.Printf("key: %s,value: %s\n", key, value)
	}
}
```

{% embed url="https://play.golang.org/p/g8iSBFN1ggn" caption="在线例子：通过range遍历字典" %}

以上代码的执行结果：

```text
key: key1,value: value1
key: key2,value: value2
```



