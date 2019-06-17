# 协程

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

