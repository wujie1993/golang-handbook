# Gin

用Go（Golang）编写的HTTP Web框架。它具有类似Martini的API，具有更好的性能。

## 项目地址

{% embed url="https://github.com/gin-gonic/gin" %}

## 快速开始

下载和安装

```text
go get -u -v github.com/gin-gonic/gin
```

例子：简单的Gin服务

```go
package main

import (
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()
	r.GET("/hello", func(c *gin.Context) {
		c.JSON(200, gin.H{
			"say": "hello",
		})
	})
	r.Run("localhost:8080")
}
```

执行`go run main.go`启动服务

服务端输出

```text
[GIN-debug] [WARNING] Creating an Engine instance with the Logger and Recovery middleware already attached.

[GIN-debug] [WARNING] Running in "debug" mode. Switch to "release" mode in production.
 - using env:   export GIN_MODE=release
 - using code:  gin.SetMode(gin.ReleaseMode)

[GIN-debug] GET    /hello                    --> main.main.func1 (3 handlers)
[GIN-debug] Listening and serving HTTP on localhost:8080
```

开启一个新的控制台，执行`curl 127.0.0.1:8080/hello`

客户端输出

```text
{"say":"hello"}
```

服务端输出

```text
[GIN] 2019/06/25 - 11:54:41 |?[97;42m 200 ?[0m|            0s |       127.0.0.1 |?[97;44m GET     ?[0m /hello
```

