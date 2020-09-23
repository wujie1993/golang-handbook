# 上下文

协程有一个运行特点，是其本身不受其他协程的影响，在编码上如果处理不当，有可能会出现一直驻留在内存区的协程，形成僵尸协程。随着运行时间的迁移，僵尸协程不断增多，会进而影响到系统的稳定性。

在内置包`context`中，有一种Context结构可以用于应对这种场景，实现对于协程的批量管理。其原理是在主方法创建一个Context对象，通过该对象再派生出子Context对象，子Context对象可以包含像是超时时间，截止时间，终止方法，携带参数等属性。

Context是一个协程安全的类型，可以将同一个Context传递给不同的协程方法使用。

将派生出的子Context以参数的形式传入协程方法中，协程中的任务在循环执行的间隔期不断地通过select语句检查该子Context状态的Done通道，如果接收到信号则说明上下文被关闭，当前协程应该退出了，否则会继续执行协程中的任务。

在主方法中，当确认协程需要退出时，会调用Context创建时产生的Cancel\(\)方法，这时该Context和该Context所派生的每个子Context都会在其Done通道中接收到结束信号，协程获取到该结束信号后，就可以终止运行退出协程。

select命令的语法与switch相似，区别是select语法中是判断哪个case的通道最先出队，当所有case通道都阻塞时，整个select语句块也会同样阻塞。为了避免select出现永久阻塞，可以加default语句块，表示在所有的case都无法出队时则运行default中的逻辑。

```text
select {
case <-chan1:
    ...
case val := <-chan2:
    ...
default:
    ...
}
```

例子：通过上下文管理协程

```go
package main

import (
	"context"
	"fmt"
	"sync"
	"time"
)

// doSomething 方法中会执行一个循环任务，在每个循环执行的周期内都会检查上下文状态，
// 如果上下文接收到Done信号，则终止任务
func doSomething(ctx context.Context, waitgroup *sync.WaitGroup) {
	// 结束方法时释放一个协程计数器
	defer func() {
		waitgroup.Done()
	}()

	fmt.Println("start doing jobs.")
	// 执行工作任务，用时6秒
	for i := 0; i < 6; i++ {
		// 执行任务
		fmt.Printf("doing job %d\n", i)
		time.Sleep(time.Second)
		// 检查上下文状态，如果接收到Done信号则终止任务，否则进入下一个循环
		select {
		case <-ctx.Done():
			fmt.Println("stop doing jobs")
			return
		default:
		}
	}
	fmt.Println("finish jobs.")
}

func main() {
	fmt.Println("Hello World")

	// 初始化等待组，用于阻塞主方法直到所有协程执行完毕
	var waitgroup sync.WaitGroup

	// 添加一个协程计数器
	waitgroup.Add(1)

	// 首先在主方法体中通过context.Background()创建一个上下文，做为根级上下文，只用于派生子上下文，不做其他用途
	// 通过根级上下文进行派生，获得一个子上下文以及子上下文的终止方法
	ctx, cancel := context.WithCancel(context.Background())

	// 执行协程方法，并将上下文和等待组做为参数传入，该方法运行耗时6秒
	go doSomething(ctx, &waitgroup)

	// 等待3秒后执行子上下文的终止方法，关闭上下文
	go func() {
		fmt.Println("stop doing jobs in 3 seconds")
		time.Sleep(3 * time.Second)
		cancel()
	}()

	// 阻塞主方法直到协程计数器归零
	waitgroup.Wait()

	fmt.Println("Finished Execution")
}

```

以上代码的执行结果

```text
Hello World
stop doing jobs in 3 seconds
start doing jobs.
doing job 0
doing job 1
doing job 2
doing job 3
stop doing jobs
Finished Execution
```



