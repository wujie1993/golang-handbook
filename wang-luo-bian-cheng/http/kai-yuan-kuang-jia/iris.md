# Iris

Iris是这个地球上通过社区驱动的最快网络框架，支持MVC，HTTP/2等特性。为每个人提供免费的技术支持。

## 官方网站

{% embed url="https://iris-go.com/" %}

## 项目地址

{% embed url="https://github.com/kataras/iris" %}

## 快速开始

下载与安装

```bash
go get -u -v github.com/kataras/iris
```

例子：简单的Iris服务

```go
package main

import "github.com/kataras/iris"

func main() {
    app := iris.Default()
    app.Get("/hello", func(ctx iris.Context) {
        ctx.JSON(iris.Map{
            "say": "hello",
        })
    })
    // listen and serve on http://localhost:8080.
    app.Run(iris.Addr("localhost:8080"))
}
```

运行命令`go run main.go`启动服务

服务端输出

```text
Now listening on: http://localhost:8080
Application started. Press CTRL+C to shut down.
```

开启一个新的控制台，执行`curl 127.0.0.1:8080/hello`

客户端输出

```text
{"say":"hello"}
```

服务端输出

```text
[INFO] 2019/06/25 15:06 200 1.0001ms 127.0.0.1 GET /hello
```

