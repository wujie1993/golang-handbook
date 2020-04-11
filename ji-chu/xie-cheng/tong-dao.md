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

### 关闭通道

通道本身并不提供关闭通道的方法，而是通过系统内置的close\(\)方法关闭通道。在通道关闭后再继续往通道写数据会引发panic错误，而读数据则可以持续读取到通道中没有值为止，读取通道支持多参数返回，其中第一个参数表示从通道中读出的值，第二个参数表示是否正确读出数据，在通道关闭的情况下会返回false，可以借此判断通道是否已经关闭

例子：关闭通道

```text
package main

import (
	"fmt"
)

func main() {

	// 创建一个通道
	msgChan := make(chan string, 10)

	// 先往通道写入一条记录
	msgChan <- "a normal message"
	
	// 关闭通道
	close(msgChan)

	// 持续从通道中读取数据，ok值为false时，表示通道已经关闭且没有数据
	for {
		msg, ok:= <-msgChan
		if !ok {
			fmt.Println("chan already closed")
			return
		}
		fmt.Printf("message: %s\n", msg)
	}
}
```

### 单向通道

通常情况下通道是双向的，可读也可写。除此之外，出于安全性考虑，还分出了只读通道和只写通道两种单向通道。单向可以由双向通道转换，但不能转换回双向通道。

定义只读通道：

```text
var <-chan {通道类型}
```

定义只写通道：

```text
var chan<- {通道类型}
```

例子：使用单向通道

```go
package main

import (
	"fmt"
	"sync"
)

// writeChan 将数据写入单向通道
func writeChan(wc chan<- string, wg *sync.WaitGroup) {
	wc <- "hello"
	wg.Done()
}

// readChan 从单向通道中读取数据并输出
func readChan(rc <-chan string, wg *sync.WaitGroup) {
	fmt.Println(<-rc)
	wg.Done()
}

func main() {
	var waitGroup sync.WaitGroup

	var c chan string = make(chan string)

	waitGroup.Add(2)
	// 将双向通道转为只读通道
	go readChan(c, &waitGroup)
	// 将双向通道转为只写通道
	go writeChan(c, &waitGroup)

	waitGroup.Wait()
}

```

{% embed url="https://play.golang.org/p/9C\_r3zEqSPV" caption="例子：使用单向通道" %}

以上代码的运行结果：

```text
hello
```

空间大小为0的通道只有在读和写同时进行的情况下两端才会解除阻塞，而在并发场景中，并发量往往不是固定不变的，有时候并发量提升，写通道压力增大，读通道来不及消费，导致越来越多的写通道协程阻塞，内存持续增长，到了一定程度，就会触发系统OOM，进而影响到服务器稳定性。对于高并发的场景，可以提高通道的大小，将无法即时消费的写请求缓存到通道中，释放协程资源。



