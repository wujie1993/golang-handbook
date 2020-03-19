# Echo

高性能，极简主义的Go Web框架

## 官方网站

{% embed url="https://echo.labstack.com/" %}

## 项目地址

{% embed url="https://github.com/labstack/echo" %}

## 快速开始

Echo建议的使用方式是Gomod，因此我们需要把项目建立在`$GOPATH`之外

例子：简单的Echo服务

{% tabs %}
{% tab title="main.go" %}
```go
package main

import (
	"net/http"

	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
)

func main() {
	// Echo instance
	e := echo.New()

	// Middleware
	e.Use(middleware.Logger())
	e.Use(middleware.Recover())

	// Routes
	e.GET("/", hello)

	// Start server
	e.Logger.Fatal(e.Start("localhost:8080"))
}

// Handler
func hello(c echo.Context) error {
	return c.String(http.StatusOK, "Hello, World!")
}

```
{% endtab %}

{% tab title="go.mod" %}
```go
module echohello

go 1.12

require github.com/labstack/echo/v4 v4.1.6

replace (
	golang.org/x/crypto v0.0.0-20190308221718-c2843e01d9a2 => github.com/golang/crypto v0.0.0-20190308221718-c2843e01d9a2
	golang.org/x/crypto v0.0.0-20190605123033-f99c8df09eb5 => github.com/golang/crypto v0.0.0-20190605123033-f99c8df09eb5
	golang.org/x/net v0.0.0-20190311183353-d8887717615a => github.com/golang/net v0.0.0-20190311183353-d8887717615a
	golang.org/x/net v0.0.0-20190404232315-eb5bcb51f2a3 => github.com/golang/net v0.0.0-20190404232315-eb5bcb51f2a3
	golang.org/x/net v0.0.0-20190607181551-461777fb6f67 => github.com/golang/net v0.0.0-20190607181551-461777fb6f67
	golang.org/x/sync v0.0.0-20190423024810-112230192c58 => github.com/golang/sync v0.0.0-20190423024810-112230192c58
	golang.org/x/sys v0.0.0-20190215142949-d0b11bdaac8a => github.com/golang/sys v0.0.0-20190215142949-d0b11bdaac8a
	golang.org/x/sys v0.0.0-20190222072716-a9d3bda3a223 => github.com/golang/sys v0.0.0-20190222072716-a9d3bda3a223
	golang.org/x/sys v0.0.0-20190412213103-97732733099d => github.com/golang/sys v0.0.0-20190412213103-97732733099d
	golang.org/x/sys v0.0.0-20190602015325-4c4f7f33c9ed => github.com/golang/sys v0.0.0-20190602015325-4c4f7f33c9ed
	golang.org/x/sys v0.0.0-20190609082536-301114b31cce => github.com/golang/sys v0.0.0-20190609082536-301114b31cce
	golang.org/x/text v0.3.0 => github.com/golang/text v0.3.0
	golang.org/x/text v0.3.2 => github.com/golang/text v0.3.2
	golang.org/x/tools v0.0.0-20180917221912-90fa682c2a6e => github.com/golang/tools v0.0.0-20180917221912-90fa682c2a6e
	golang.org/x/tools v0.0.0-20190608022120-eacb66d2a7c3 => github.com/golang/tools v0.0.0-20190608022120-eacb66d2a7c3
)

```
{% endtab %}
{% endtabs %}

执行`go run main.go`将依赖包下载到本地并运行服务

服务端输出

```text

   ____    __
  / __/___/ /  ___
 / _// __/ _ \/ _ \
/___/\__/_//_/\___/ v4.1.5
High performance, minimalist Go web framework
https://echo.labstack.com
____________________________________O/_______
                                    O\
⇨ http server started on 127.0.0.1:8080
```

开启一个新的控制台执行`curl 127.0.0.1:8080`

客户端输出

```text
Hello, World!
```

服务端输出

```text
{"time":"2019-06-25T14:18:21.7213805+08:00","id":"","remote_ip":"127.0.0.1","host":"127.0.0.1:8080","method":"GET","uri":"/","user_agent":"curl/7.65.1","status":200,"error":"","latency":0,"latency_human":"0s","bytes_in":0,"bytes_out":13}
```

