# 字符串

字符串类型关键字`string`

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

字符串类型只能整体重新赋值，无法修改其中的某一位字符，需要改变字符串中的某个元素值时，可以先转换为`[]byte`或`[]rune`后，改变数组中对应下标的元素，再转换为一个新的字符串

例子：字符串转换

```go
package main

import (
    "fmt"
)

func main() {
    var s1 = "hello 你好"
    var s2 = []byte(s1)
    var s3 = []rune(s1)

    s2[0] = 'k'
    s3[6] = '我'
    
    fmt.Println(string(s2))
    fmt.Println(string(s3))
}
```

以上的代码执行结果：

```bash
kello 你好
hello 我好
```

