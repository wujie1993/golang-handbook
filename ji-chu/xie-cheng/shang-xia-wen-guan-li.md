# 上下文

协程有一个运行特点，是其本身不受其他协程的影响，在编码上如果不留神，有可能会出现一直运行驻留在内存区的协程，形成僵尸协程。随着运行时间的迁移，僵尸协程不断增多，会进而影响到系统的稳定性。

在内置包`context`中，有一种`Context`结构可以用于应对这种场景，实现对于协程的批量管理。其原理是在主方法创建一个context，并将其传入协程中，协程中会不断地的检查该`context`状态，如果是`Done`则说明协程应该退出了，否则会继续执行协程中的任务。在主方法中，当确认协程需要退出时，会调用`context`产生的`Cancel()`方法，这会给其之下的每个协程中的`Context`发送信号，将状态置为`Done`，协程们发现`Context`为`Done`状态时，就可以退出协程。

例子：通过上下文管理协程

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func doSomething(ctx context.Context) {
	// 执行工作任务
	for i := 0; i < 6; i++ {
		// 检查上下文状态，如果非Done继续执行工作，Done则停止并跳出方法
		select {
		case <-ctx.Done():
			fmt.Println("stop doing jobs")
			return
		default:
			// 执行工作
			time.Sleep(time.Second)
			fmt.Println("do some jobs")
		}
	}
	fmt.Println("finish jobs.")
}

func main() {
	fmt.Println("Hello World")

	// 创建上下文
	ctx, cancel := context.WithCancel(context.Background())

	// 执行协程，传入上下文
	go doSomething(ctx)

	// 等待3秒后关闭上下文
	go func() {
		time.Sleep(3 * time.Second)
		cancel()
	}()

	// 等待5秒后关闭上下文
	time.Sleep(5 * time.Second)

	fmt.Println("Finished Execution")
}
```

{% embed url="https://play.golang.org/p/gf7Cmo5yOeU" caption="在线例子：通过上下文管理协程" %}

以上代码的执行结果

```text
Hello World
do some jobs
do some jobs
do some jobs
do some jobs
stop doing jobs
Finished Execution
```



