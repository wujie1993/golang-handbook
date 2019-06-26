# 性能分析

当我们把代码写完，单元测试，性能测试都做了，程序也能够运行起来，一切看起来都很美好。然而，光有这些是不足以保证服务能够长久稳定运行的，一些问题只有在长时间运行之后才会发现，比方说一些网络连接没有释放或者是协程陷入了阻塞或死循环，这些问题会在运行的过程中慢慢累积，到达某一瓶颈时爆发，影响到服务器系统的稳定性。

在golang中提供了对于应用程序运行阶段进行性能分析的工具pprof，根据场景的不同分为了两个库：`runtime/pprof`和`net/http/pprof`。

`runtime/pprof`用于为非守护形式运行的程序做性能分析

`net/http/pprof`用于为守护态的http服务程序做性能分析

接下来我们以一个http服务的例子来看如何做性能分析

例子：http服务性能分析

```go
package main

import (
	"fmt"
	"net/http"
	_ "net/http/pprof"

	"time"
)

func slow(w http.ResponseWriter, req *http.Request) {
	time.Sleep(time.Second * 5)
	w.Write([]byte("a slow return"))
}

func fast(w http.ResponseWriter, req *http.Request) {
	w.Write([]byte("a fast return"))
}

func endless(w http.ResponseWriter, req *http.Request) {
	for {
		time.Sleep(time.Second)
		if false {
			break
		}
	}
	w.Write([]byte("you will never see this message"))
}

func main() {
	http.HandleFunc("/slow", slow)

	http.HandleFunc("/fast", fast)

	http.HandleFunc("/endless", endless)

	if err := http.ListenAndServe("localhost:8080", nil); err != nil {
		fmt.Println(err)
	}
}

```

执行go run main.go启动服务

开启浏览器访问http://localhost:8080/debug/pprof/可以看到如下内容

![](../../.gitbook/assets/image%20%286%29.png)

可以看到各项指标的当前运行统计数据，点击各项指标的链接可以查看到指标的当前详情

接下来使用go tool pprof交互式工具统计服务运行60秒间的性能情况

再开启一个新的控制台，执行命令

```text
go tool pprof http://localhost:8080/debug/pprof/profile?seconds=60
```

输出如下

```text
Fetching profile over HTTP from http://localhost:8080/debug/pprof/profile?seconds=60
```

这时候可以分别多次访问以下测试链接

```text
http://localhost:8080/fast
http://localhost:8080/slow
http://localhost:8080/endless
```

60秒后交互式工具控制台会输出如下内容

```text
Saved profile in C:\Users\wj361\pprof\pprof.samples.cpu.001.pb.gz
Type: cpu
Time: Jun 26, 2019 at 10:46am (CST)
Duration: 60s, Total samples = 30ms ( 0.05%)
Entering interactive mode (type "help" for commands, "o" for options)
(pprof)
```

说明分析报告已经生成完毕，可以输入help+Enter键查看支持的命令

比分说查看cpu开销前10的方法

```text
(pprof) top10
Showing nodes accounting for 30ms, 100% of 30ms total
Showing top 10 nodes out of 15
      flat  flat%   sum%        cum   cum%
      10ms 33.33% 33.33%       10ms 33.33%  runtime.memmove
      10ms 33.33% 66.67%       10ms 33.33%  runtime.stdcall1
      10ms 33.33%   100%       10ms 33.33%  runtime.timerproc
         0     0%   100%       10ms 33.33%  net/http.(*conn).serve
         0     0%   100%       10ms 33.33%  net/http.(*connReader).startBackgroundRead
         0     0%   100%       10ms 33.33%  runtime.entersyscallblock_handoff
         0     0%   100%       10ms 33.33%  runtime.handoffp
         0     0%   100%       10ms 33.33%  runtime.mstart
         0     0%   100%       10ms 33.33%  runtime.newproc
         0     0%   100%       10ms 33.33%  runtime.newproc.func1
```

如果要查看其他的指标，也是以同样的方式运行交互式工具

```text
go tool pprof http://localhost:8080/debug/pprof/{指标名称}?seconds=60
```

除了通过命令行交换工具查看分析报告外，还可以通过图形界面查看，这需要使用到graphviz工具

{% embed url="https://www.graphviz.org/download/" caption="graphviz下载地址" %}

{% hint style="info" %}
需要将graphviz的二进制目录添加到环境变量$PATH中
{% endhint %}

开启cpu性能分析并启动web视图报告

```text
go tool pprof -http=:8081 http://localhost:8080/debug/pprof/profile?seconds=60
```

分别多次访问以下测试链接

```text
http://localhost:8080/fast
http://localhost:8080/slow
http://localhost:8080/endless
```

60秒后打开浏览器访问http://localhost:8081/ui/，可以看到方法的调用链，其中框越大线越粗表示耗得时间更长

![](../../.gitbook/assets/image%20%287%29.png)

通过下拉菜单VIEW可以切换各种视图

