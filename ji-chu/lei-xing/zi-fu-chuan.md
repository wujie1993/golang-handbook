# 字符串

字符串类型关键字`string`，使用符号`""`表示字符串字面常量

例子：定义字符串

```go
var name string
name = "mike"
```

{% hint style="info" %}
字符串类型只能整体重新赋值，无法修改其中的某一位字符
{% endhint %}

{% hint style="info" %}
字符串默认采用UTF-8编码
{% endhint %}

需要改变字符串中的某个元素值时，可以先转换为`[]byte`或`[]rune`，改变数组中对应下标的元素，再转换为一个新的字符串

例子：字符串与字符转换

```go
package main

import (
    "fmt"
)

func main() {
    // 定义字符串s1
    var s1 = "hello 你好"
    // 将字符串s1格式转换为[]byte,赋值给s2
    var s2 = []byte(s1)
    // 将字符串s1格式转换为[]rune,赋值给s3
    var s3 = []rune(s1)

    // 修改s1中下标0的值
    s2[0] = 'k'
    // 修改s2中下标6的值
    s3[6] = '我'
    
    // 将s2转换为新的字符串输出
    fmt.Println(string(s2))
    // 将s3转换为新的字符串输出
    fmt.Println(string(s3))
}
```

{% embed url="https://play.golang.org/p/9O1De\_s8EHW" caption="在线例子：字符串与字符转换" %}

以上的代码执行结果：

```bash
kello 你好
hello 我好
```

内置包`strconv`中封装了关于字符串转换的方法，可通过方法`Atoi()`将字符串转为整型，通过方法`Iota()`将整型转换为字符串

例子：字符串与整型转换

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	var v1 int
	// 将字符串"2"转换为整型
	v1, _ = strconv.Atoi("2")
	fmt.Println(v1)

	var v2 string
	// 将整型值3转换为字符串
	v2 = strconv.Itoa(3)
	fmt.Println(v2)
}
```

{% embed url="https://play.golang.org/p/GKAOcLKZyeT" caption="在线例子：字符串与整型转换" %}

以上代码的执行结果：

```bash
2
3
```

也可以通过`ParseXXX()`将字符串转换为其他类型的变量，通过`FormatXXX()`将其他类型的变量转换为字符串（`XXX`代指类型）

例子：字符串的多种类型转换

```text

```
