# 等待组

通过time.Sleep\(\)这种方式去等待协程执行完毕是不可靠的，我们大多数情况下无法估计协程的运行时长，需要有一个能够准确获取到协程结束运行的方法。

在内置包`sync`中有一个WaitGroup类型可以应对这种场景，其通过锁机制实现。其原理是在WaitGroup中有一个带锁的计数器，通过其Add\(\)方法可以为计数器累加值；还有一个Done\(\)方法，用于释放释放计数器值\(减1\)；还有一个Wait\(\)方法，该方法会一直阻塞直到计数器归零。

于是就可以这么做，在协程执行前为WaitGroup计数器加1，接着把WaitGroup做为参数传入并运行协程，在协程终止时执行WaitGroup的Done\(\)方法释放计数器。在主方法体中执行WaitGroup的Wait\(\)方法阻塞直到WaitGroup的计数器归零。这样只有当所有协程都执行结束时，计数器才会归零，主方法体也才能解除阻塞状态继续往下运行。

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

以上代码的执行结果：

```text
Hello World
Inside my goroutine
Finished Execution
```

