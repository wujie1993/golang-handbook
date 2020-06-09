# 字典

## 字典定义

字典用于存储键值对数据，关键字`map`，定义格式：

```text
var 变量名 map[键类型]值类型
```

字典在未赋值的情况下值为`nil`,通过`make()`方法初始化字典

## 字典初始化

例子：初始化字典

```go
// 定义一个键为字符串，值为字符串的字典m1
var m1 map[string]string
// 初始化字典m1
m1 = make(map[string]string)

// 定义一个键为字符串，值为整型切片字典m2
var m2 map[string][]int
// 初始化字典m2
m2 = make(map[string][]int)
```

## 字典元素读写

字典内容可动态增删，通过键名可对字典进行索引，做赋值或读取操作，使用`len()`方法可获取字典长度

例子：字典读写

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
	
	// 通过键名获取对应的值
	fmt.Println(m1["key2"])

	// 输出字典内容
	fmt.Println(m1)
	// 输出字典长度
	fmt.Println(len(m1))
}
```

以上代码的执行结果：

```text
value2
map[key1:value1 key2:value2]
2
```

## 字典元素删除

可通过内置方法delete\(\)删除字典元素

例子：删除字典元素

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
	
	// 通过键名删除字典元素
	delete(m1,"key1")

	// 输出字典内容
	fmt.Println(m1)
	// 输出字典长度
	fmt.Println(len(m1))
}
```

以上代码的执行结果：

```text
map[key2:value2]
1
```





