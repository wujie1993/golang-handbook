# 错误处理

## error接口

golang中定义了错误接口`error`，其中包含了错误方法`Error() string`，可以通过内置包`errors`中的`NewError()`方法创建一个错误

例子：引发一个错误

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	// 调用一个会返回错误的方法，判断err是否为空，非空则输出错误
	if err := BadFunc(); err != nil {
		fmt.Println(err)
	} else {
		fmt.Println("everything is fine")
	}
}

func BadFunc() error {
	// 通过errors.New()方法创建并返回一个错误
	return errors.New("a bad error")
}
```

以上代码的执行结果：

```text
a bad error
```

## 自定义错误

只要实现了`Error() string`方法，就可以做为错误接口的传递参数，引发自定义的错误

例子：自定义错误

```go
package main

import (
	"fmt"
)

// 定义一个自定义错误结构
type MyErr struct{}

// 实现error接口的Error()方法
func (e MyErr) Error() string {
	return "Sorry, this is my fault."
}

func main() {
	// 定义一个实现error接口的变量err
	var err error
	// 为变量初始化并赋值为自定义错误
	err = MyErr{}

	// fmt方法中会判断变量是否实现了Error接口，如果实现的话会输出Error()方法的内容
	fmt.Println(err)
}
```

以上代码的执行结果：

```text
Sorry, this is my fault.
```

## panic

除了通过`error`接口报普通错误外，在一些比较严重的场合可以用内置的`panic()`方法引发一个运行时错误，运行时错误会在方法中逐层往上传递，直到去到`main()`方法中，引起程序的错误退出

### 引发错误

例子：引发panic错误

```go
package main

func main() {
	panic("oh!no!!!")
}
```

以上代码的运行结果：

```text
panic: oh!no!!!

goroutine 1 [running]:
main.main()
	/tmp/sandbox037140472/prog.go:4 +0x40
```

### 错误处理

在一些代码库中，发送严重错误后会引发`panic()`导致程序退出，当已经有办法处理这个错误时，我们不希望程序退出而是将其做为普通错误处理，这时就需要能够拦截`panic()`错误的传递。可以通过内置的`recover()`方法结合延迟执行`defer`，实现对于`panic()`的错误处理

例子：错误处理

```go
package main

import (
	"fmt"
)

func main() {
	// 在main方法执行的末尾执行
	defer func() {
		// 拦截panic错误并将错误赋值给e
		e := recover()
		// 通过类型断言e是否实现了error接口，分别输出错误
		if err, ok := e.(error); ok {
			fmt.Println(err)
		} else {
			fmt.Println(e)
		}
	}()

	// 引发一个panic错误
	panic("oh!no!!!")
}
```

以上代码运行结果：

```text
oh!no!!!
```

