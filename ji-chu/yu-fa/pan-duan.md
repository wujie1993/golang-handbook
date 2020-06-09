# 判断

条件判断有两种方式，`if`和`switch`

## if判断

if 语句的语法结构：

```text
if 条件判断 {
    ...
}else if 条件判断 {
    ...
}else {
    ...
}
```

例子：If条件判断

```go
package main

import (
	"fmt"
)

func main() {
	CheckScore(59)

	CheckScore(65)

	CheckScore(88)
}

func CheckScore(score int) {
	// 通过if语句判断不同score范围时的输出信息
	if score < 60 {
		fmt.Println("bad")
	} else if score >= 60 && score < 80 {
		fmt.Println("normal")
	} else {
		fmt.Println("good")
	}
}
```

以上代码的执行结果：

```text
bad
normal
good
```

## switch判断

`switch`可以用于进行逻辑判断，其中包含了各种`case`。`switch`会顺序判断`case`是否成立，当`case`条件成立时会执行`case`体中的内容。当所有的`case`都不成立时，会执行`default`体中的内容。可以通过关键字`fallthrough`连带执行下一个`case`体中的内容，不管`case`条件是否成立。在必要时可以通过关键字`break`跳出`switch`语句

无判断变量的`switch`语句会逐个遍历`case`体中的条件判断按顺序进行比较，如果条件成立时就会执行其中的逻辑，语法结构：

```text
switch {
case 条件判断1:
    ...
case 条件判断2:
    ...
default:
    ...
}
```

例子：无判断变量的switch语句

```go
package main

import (
	"fmt"
)

func main() {
	CheckScore(59)

	CheckScore(65)

	CheckScore(88)
}

func CheckScore(score int) {
	// 通过switch语句判断不同score范围时的输出信息
	switch {
	case score < 60:
		fmt.Println("bad")
	case score >= 60 && score < 80:
		fmt.Println("normal")
	default:
		fmt.Println("good")
	}
}
```

以上代码的执行结果：

```text
bad
normal
good
```

有判断变量的`switch`语句用于进行确切值的判断，会逐个与其中的`case`常量按顺序进行比较，如果值相同就会执行其中的逻辑，语法结构：

```text
switch 变量 {
case 常量1:
    ...
case 常量2:
    ...
default:
    ...
}
```

例子：有判断变量的switch条件判断

```go
package main

import (
	"fmt"
)

func main() {
	switchColor("red")

	switchColor("yellow")

	switchColor("blue")

	switchColor("black")
}

func switchColor(color string) {
	// 通过switch语句判断不同color时的输出信息
	switch color {
	case "yellow":
		fmt.Println("banana is yellow.")
		// 使用fallthrough可以连带执行下一个case体中的逻辑
		fallthrough
	case "red":
		fmt.Println("apple is red.")
	case "blue":
		fmt.Println("sky is blue.")
		// 使用break跳出switch体
		if true {
			break
		}
		fmt.Println("you will never see me ")
	default:
		fmt.Println("i have no idea what color it is.")

	}
}
```

以上代码的执行结果：

```text
apple is red.
banana is yellow.
apple is red.
sky is blue.
i have no idea what color it is.
```



