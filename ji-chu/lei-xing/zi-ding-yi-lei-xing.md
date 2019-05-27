# 自定义类型

可以将各种内置类型重新定义命名为新的类型名称，该类型就称为自定义类型

例子：定义自定义类型

```go
package main

import (
	"fmt"
)

// 通过int64自定义Long类型
type Long int64

func main() {
	// 通过自定义类型Long定义并赋值变量long
	var long Long = 231231231
	fmt.Println(long)
}
```

{% embed url="https://play.golang.org/p/a1U-gLjEzgw" caption="在线例子：定义自定义类型" %}

以上代码的执行结果：

```text
231231231
```



