# 协程

## 协程

协程可以理解为轻量级的线程，得益于golang的优化，协程的数量可以轻松创建出成千上万个

协程可以用于应对一些高并发场景，如api请求

通过关键字`go`作用到一个方法上去创建一个协程

例子：创建并运行协程

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	// 通过关键字go去执行一个方法时，就会根据该方法创建一个对应的协程
	go func() {
		fmt.Println("running goroutine")
	}()

	// 由于协程的执行是非阻塞的，协程创建后主方法会继续往下走，直接退出进程，这时协程很有可能还没执行到，需要有一个方法去等待协程执行完毕
	time.Sleep(time.Second)
}
```

{% embed url="https://play.golang.org/p/3grKRFPe363" caption="例子：创建并运行协程" %}

以上代码的执行结果

```text
running goroutine
```

## 等待组

然而通过`time.Sleep()`这种方式去等待协程执行完是不靠谱的，大多数情况下是无法估计协程再什么情况下会运行结束的。需要有一个能接收到`goroutine`结束信号的方式。

在内置包`sync`中有一个`WaitGroup`结构可以应对这种场景，其通过锁机制实现。其原理是`WaitGroup`有一个带计数器的锁，通过其`Add()`方法可以为计数器累加值，其中还有一个`Wait()`方法，该方法会不断判断计数器的值，一直阻塞着直到计数器值为0；还有一个`Done()`方法，该方法用于释放释放计数器值\(值减1\)。

于是就可以这么做，在`gorotine`执行前为`WaitGroup`计数器加1，接着把WaitGroup做为参数传入并运行`groutine`，在`groutine`执行的末尾去执行`WaitGroup`的`Done()`方法释放计数器，在`goroutine`下方执行WaiGroup的`Wait()`方法阻塞等待计数器释放到0为止。这样只有当`goroutine`都执行结束时，计数器才会清零，`Wait()`也才能解除阻塞，主方法体才会退出

例子：使用WaitGroup等待协程退出

```go
package main

import (
	"fmt"
	"sync"
)

func myFunc(waitgroup *sync.WaitGroup) {
	fmt.Println("Inside my goroutine")
	// 释放一个计数器
	waitgroup.Done()
}

func main() {
	fmt.Println("Hello World")

	// 实例化一个等待组
	var waitgroup sync.WaitGroup

	// 等待组计数器加1
	waitgroup.Add(1)

	// 执行将等待组做为参数传入并执行goroutine
	go myFunc(&waitgroup)

	// 阻塞直到计数器清零
	waitgroup.Wait()

	fmt.Println("Finished Execution")
}
```

{% embed url="https://play.golang.org/p/M\_N\_v\_4xGSd" caption="在线例子：使用WaitGroup等待协程退出" %}

以上代码的执行结果：

```text
Hello World
Inside my goroutine
Finished Execution
```

