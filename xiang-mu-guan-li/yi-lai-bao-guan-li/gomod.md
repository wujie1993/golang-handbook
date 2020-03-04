# Gomod

## 模块初始化

以一个简单的iris网站为例

{% code title="main.go" %}
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
{% endcode %}

在当前目录中，执行以下命令进行初始化：

```text
go mod init
```

或者指定模块名：

```text
go mod init {{ module名称 }}
```

在不指定module名称的时候默认使用目录名作为module名称

初始化完成后会在当前目录中生成一个go.mod文件，格式如下：

{% code title="go.mod" %}
```go
module {{ module名称 }}

go {{ golang版本 }}
```
{% endcode %}

使用以下命令生成包依赖并下载依赖包到$GOPATH中

```text
go get -u -v
```

依赖包下载后即可执行go build或go run。也可以在还未执行go get的情况下执行go build或go run，这两个指令会事先检查go.mod和本地的依赖包，如果存在缺少依赖包的情况会自动进行go get。

gomod获取依赖包的版本策略是优先获取最新的release，在没有release存在的情况下会获取最新的commit。

