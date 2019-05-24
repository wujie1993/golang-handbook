# 复数

复数包含两个部分，实部和虚部。复数的表示形式为：

$$
a + bi
$$

`a`为实部，使用浮点数表示

`b`为虚部，使用浮点数表示

`i`为虚数单位，公式为：

$$
i ^ 2 =  -1
$$

| 类型 | 长度（byte） | 实部长度（byte） | 虚部长度（byte） |
| :--- | :--- | :--- | :--- |
| complex64 | 8 | 4（同float32） | 4（同float32） |
| complex128 | 16 | 8（同float64） | 8（同float64） |

可通过内置方法`complex()`定义复数，通过`real()`方法获取复数的实部，`imag()`获取复数的虚部

例子：定义与输出复数

```go
package main

import (
	"fmt"
)

func main() {
	var v1 complex64
	v1 = 2.7 + 34i

	var v2 complex64
	// 使用内置方法complex(x,y)定义复数，x表示实部，y表示虚部
	v2 = complex(2.7, 34)

	fmt.Println(v1)
	// 通过内置方法real()获取复数实部，imag()获取复数虚部实数
	fmt.Println(real(v2), imag(v2))
}
```

{% embed url="https://play.golang.org/p/twT-YInCRn3" caption="在线例子：定义与输出复数" %}

以上例子的输出结果：

```bash
(2.7+34i)
2.7 34
```

