# 浮点型

浮点型表示数字浮点数

| 类型 |  |
| :--- | :--- |
| float32 |  |
| float64 |  |

例子： 定义浮点型

```go
var speed float32
speed = 35.75
```

{% hint style="info" %}
在对浮点数进行匿名赋值的时候，默认会识别为`float64`类型
{% endhint %}

{% hint style="info" %}
浮点型不是绝对精确的数值类型，存在一定的误差，因此在进行浮点数的比较时，需要指定精度
{% endhint %}

例子：存在误差的浮点数比较

```go
package main

import (
	"fmt"
)

func main() {
	var x, y = 3.1, 3.0
	fmt.Println(x-y == 0.1)
}
```

以上代码的执行输出结果会是`false`

例子：根据精度比较浮点数

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	var x, y float64 = 3.1, 3.0
	fmt.Println(IsEqual(x-0.1, y, 0.1))
}

// IsEqual 判断x是否大于y,precision为判断精度
func IsEqual(x float64, y float64, precision float64) bool {
	return math.Abs(x-y) < precision
}
```

以上代码的输出结果会是`true`

