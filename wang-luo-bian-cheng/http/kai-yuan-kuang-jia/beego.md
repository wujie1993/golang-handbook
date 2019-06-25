# Beego

beego是用于快速开发RESTFUL API，web应用和后端服务。灵感来自于Tornado，Sinatra和Flask。beego具有一些特定于Go的功能，例如接口和结构嵌入。

## 官方网站

{% embed url="https://beego.me/" %}

## 项目地址

{% embed url="https://github.com/astaxie/beego" %}

## 快速开始

下载和安装

```text
go get -u github.com/astaxie/beego
```

例子：简单的beego服务

```go
package main

import (
	"github.com/astaxie/beego"
)

type HelloController struct {
	beego.Controller
}

func (c HelloController) SayHello() {
	c.SetData(map[string]string{"say": "hello"})
	c.ServeJSON()
}

func main() {
	beego.Router("/hello", &HelloController{}, "get:SayHello")
	beego.Run("localhost:8080")
}
```

执行`go run main.go`启动服务

开启一个新的控制台执行命令`curl 127.0.0.1:8080/hello`

客户端返回结果

```text
{"say":"hello"}
```

