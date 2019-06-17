# 通道

通道关键字`chan`是设计用于解决协程间通信的一种协程安全的类型，可以理解为一种空间大小固定，可以跨协程使用的队列。定义通道的语法：

```go
var {变量名} chan {通道类型}
```

通过`make()`方法创建通道

例子：定义并初始化一个通道

```go
// 定义一个字符串类型的消息通道
var msgChan chan string
// 为通道分配内存空间，表示通道中可缓存的数据量，不指定时默认为0
msgChan = make(chan string,10)
```

通道可以做为参数在协程间传递，通过操作符`<-`和`->`将数据读出通道或写入通道

例子：使用通道等待协程结束

```go
package main

import (
	"fmt"
)

func myFunc(msgChan chan bool) {
	fmt.Println("Inside my goroutine")
	// 将数据写入通道，此处阻塞直到另一端读取通道
	msgChan <- true
}

func main() {
	fmt.Println("Hello World")

	// 创建一个通道
	var msgChan chan bool = make(chan bool)

	// 将通道做为参数传入并执行协程
	go myFunc(msgChan)

	// 从通道中读取数据，此处会阻塞直到另一端写入通道
	<-msgChan

	fmt.Println("Finished Execution")
}
```

{% embed url="https://play.golang.org/p/LvQNj4WGWUW" caption="在线例子：使用通道等待协程结束" %}

以上代码执行结果：

```text
Hello World
Inside my goroutine
Finished Execution
```



