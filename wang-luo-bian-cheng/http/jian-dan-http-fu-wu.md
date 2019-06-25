# http服务基础

## Handler

内置包net/http提供了一系列与http相关的方法，包括服务端与客户端。通过http.ListenAndServe\(\)方法侦听端口并启动http服务，当服务通过端口接收到请求后，需要根据http的请求路径将该请求分配给指定的处理方法，http.HandleFunc\(\)实现了请求路径与handler方法的绑定，由handler方法完成请求的处理和响应。

例子：使用http.HandleFunc\(\)处理http请求

```go
package main

import (
	"fmt"
	"net/http"
)

// HelloServer 一个http.HandlerFunc，响应hello,world!
func HelloServer(w http.ResponseWriter, req *http.Request) {
	w.Write([]byte("hello,world!\n"))
}

func main() {
	// 将所有匹配到路径'/'的请求交由HelloServer方法处理
	http.HandleFunc("/", HelloServer)
	// 开启http服务并侦听到8080端口上，在不填地址的情况下会侦听所有地址
	if err := http.ListenAndServe(":8080", nil); err != nil {
		fmt.Println(err)
	}
}
```

执行命令`go run main.go`启动http服务，此时控制台会阻塞挂起

开启一个新的命令行控制台，执行`curl 127.0.0.1:8080`访问http服务，输出结果为

```text
hello,world!
```

除了http.HandleFunc\(\)外，也可以用http.Handle\(\)完成相同的事情，区别是http.HandleFunc\(\)中第二个参数是一个方法，而http.Handle\(\)中是一个http.Handler接口，http.Handler接口定义如下

```go
type Handler interface {
        ServeHTTP(ResponseWriter, *Request)
}
```

在http.Handle\(\)方法中会将请求交由http.Handler接口中的ServeHTTP\(\)方法处理并响应，需要在自定义的类型中实现ServeHTTP\(\)方法

例子：使用http.Handle\(\)处理http请求

```go
package main

import (
	"fmt"
	"net/http"
)

// HelloServer 实现了http.Handler接口，可以用于处理http请求
type HelloServer struct {
}

// ServeHTTP 用于处理http请求并响应结果,返回hello,world!
func (s HelloServer)ServeHTTP(res http.ResponseWriter, req *http.Request) {
	res.Write([]byte("hello,world!\n"))
}

func main() {
	// 将对于路径/下的请求交由实现了http.Handler接口的一个HelloServer对象处理
	http.Handle("/", HelloServer{})
	if err := http.ListenAndServe(":8080", nil); err != nil {
		fmt.Println(err)
	}
}
```

执行命令`go run main.go`启动http服务

开启一个新的命令行控制台，执行`curl 127.0.0.1:8080`访问http服务，输出结果为

```text
hello,world!
```

在方法http.ListenAndServe\(\)中，第一个参数为监听地址；第二个参数表示服务端处理程序， 通常为空，这意味着服务端调用http.DefaultServeMux进行处理。http.DefaultServeMux是默认的路由分发器，而服务端编写的业务逻辑处理程序http.Handle\(\)或http.HandleFunc\(\) 默认将路由注入 http.DefaultServeMux 中。

## ServeMux

除了使用默认的路由分发器之外，还可以使用自定义的路由分发器。

首先，通过http.NewServeMux\(\)方法就可以创建一个自定义的路由分发器，在该分发器里面也封装了Handle\(\)和HandleFunc\(\)方法，在调用http.ListenAndServe\(\)方法时，将路由分发器做为第二个参数传入即可生效。

例子：自定义ServerMux

```go
package main

import (
	"fmt"
	"net/http"
)

// 以func(http.responseWriter,*http.Request)格式实现了一个http请求的handler方法
func HelloServer(w http.ResponseWriter, req *http.Request) {
	w.Write([]byte("hello,world!\n"))
}

func main() {
	// 创建一个自定义路由分发器
	mux := http.NewServeMux()
	// 将所有匹配到路径'/'的请求交由HelloServer方法处理并注入到自定义路由分发器中
	mux.HandleFunc("/", HelloServer)
	// 开启http服务并侦听到8080端口上，所有的请求都交由自定义路由分发器处理
	if err := http.ListenAndServe(":8080", mux); err != nil {
		fmt.Println(err)
	}
}
```

执行命令`go run main.go`启动http服务

开启一个新的命令行控制台，执行`curl 127.0.0.1:8080`访问http服务，输出结果为

```text
hello,world!
```

## Server

http.ListenAndServe会使用默认配置开启一个Server，与ServeMux一样，Server也是可以自定义的。通过实例化一个http.Server对象即可创建

例子：自定义Server

```go
package main

import (
	"fmt"
	"net/http"
	"time"
)

// Wait 等待6秒后返回hello,world!
func Wait(w http.ResponseWriter, req *http.Request) {
	time.Sleep(6 * time.Second)
	w.Write([]byte("hello,world!\n"))
}

// Hello 返回hello,world!
func Hello(w http.ResponseWriter, req *http.Request) {
	w.Write([]byte("hello,world!\n"))
}

func main() {
	// 创建一个自定义路由分发器
	mux := http.NewServeMux()

	// 为自定义路由分发器绑定路由方法
	mux.HandleFunc("/hello", Hello)
	mux.HandleFunc("/wait", Wait)

	// 实例化一个自定义Server
	server := http.Server{
		Addr:         ":8080",
		ReadTimeout:  5 * time.Second,
		WriteTimeout: 5 * time.Second,
		Handler:      mux,
	}

	// 启动自定义Server
	if err := server.ListenAndServe(); err != nil {
		fmt.Println(err)
	}
}

```

执行命令`go run main.go`启动http服务

开启一个新的命令行控制台

执行`curl 127.0.0.1:8080/hello`输出结果

```text
hello,world!
```

执行`curl 127.0.0.1:8080/wait`输出结果

```text
curl: (52) Empty reply from server
```

## Middleware

对于http服务，不同的请求路径会对应到不同的处理方法上，如果我们需要对这些方法做日志采集或异常处理等通用的行为，在每个处理方法体中去调用这些功能的实现是比较麻烦的，而且也不够优雅。

我们可以使用装饰器模式实现一个中间层，在ServeMux路由分发器和HandlerFunc处理方法之间生效，让我们在不需要修改处理方法的情况下，为处理方法附加一些通用的功能。

例子：http处理方法中间件

```go
package main

import (
	"log"
	"net/http"
	"time"
)

// LoggerMiddleware 日志中间件，为处理方法封装一层日志输出
func LoggerMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		start := time.Now()
		log.Printf("Request %s %s", r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
		log.Printf("Response %s in %v", r.URL.Path, time.Since(start))
	})
}

// PanicMiddleware 异常处理中间件，为处理方法封装异常处理逻辑
func PanicMiddleware(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		defer func() {
			e := recover()
			if err, ok := e.(error); ok {
				log.Println(err)
				w.Write([]byte(err.Error()))
			} else {
				log.Println(e)
				w.Write([]byte("unknown error"))
			}
		}()
		next.ServeHTTP(w, r)
	})
}

// Hello 一个http.HandlerFunc处理方法,返回hello,world!
func Hello(w http.ResponseWriter, req *http.Request) {
	w.Write([]byte("hello,world!\n"))
}

// Panic 一个http.HandlerFunc处理方法，引发一个异常
func Panic(w http.ResponseWriter, req *http.Request) {
	panic("oh,we have got a panic!!!")
}

func main() {
	// 为/hello路径下的处理方法Hello套上日志输出中间件
	http.Handle("/hello", LoggerMiddleware(http.HandlerFunc(Hello)))
	// 为/panic路径下的处理方法Panic套上日志输出和异常处理中间件
	http.Handle("/panic", PanicMiddleware(LoggerMiddleware(http.HandlerFunc(Panic))))
	http.ListenAndServe("localhost:8080", nil)
}
```

服务端执行命令`go run main.go`启动http服务

开启一个新的命令行控制台

客户端执行`curl 127.0.0.1:8080/hello`

**客户端输出**

```text
hello,world!
```

**服务端输出**

```text
2019/06/25 10:46:47 Request GET /hello
2019/06/25 10:46:47 Response /hello in 16.0028ms
```

客户端执行`curl 127.0.0.1:8080/panic`

**客户端输出**

```text
unknown error
```

**服务端输出**

```text
2019/06/25 10:47:32 Request GET /panic
2019/06/25 10:47:32 oh,we have got a panic!!!
```

