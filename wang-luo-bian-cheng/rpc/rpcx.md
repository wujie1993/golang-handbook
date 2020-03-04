# RPCX

一个简单快速的rpc服务发现与治理框架

## 官方网站

{% embed url="http://rpcx.site/" %}

## 项目地址

{% embed url="https://github.com/smallnest/rpcx" %}

## 快速开始

下载与安装

```bash
go get -v github.com/smallnest/rpcx/...
```

例子：简单rpcx服务

{% tabs %}
{% tab title="protos/example.go" %}
```go
package protos

import "context"

type Args struct {
	A int
	B int
}

type Reply struct {
	C int
}

type Arith int

func (t *Arith) Mul(ctx context.Context, args *Args, reply *Reply) error {
	reply.C = args.A * args.B
	return nil
}
```
{% endtab %}

{% tab title="server/main.go" %}
```go
package main

import (
	"flag"

	example "demo/rpc/rpcx/protos"

	"github.com/smallnest/rpcx/server"
)

var (
	addr = flag.String("addr", "localhost:8972", "server address")
)

func main() {
	flag.Parse()

	s := server.NewServer()
	//s.RegisterName("Arith", new(example.Arith), "")
	s.Register(new(example.Arith), "")
	s.Serve("tcp", *addr)
}

```
{% endtab %}

{% tab title="client/main.go" %}
```go
package main

import (
	"context"
	"flag"
	"log"

	example "demo/rpc/rpcx/protos"

	"github.com/smallnest/rpcx/client"
)

var (
	addr = flag.String("addr", "localhost:8972", "server address")
)

func main() {
	flag.Parse()

	d := client.NewPeer2PeerDiscovery("tcp@"+*addr, "")
	xclient := client.NewXClient("Arith", client.Failtry, client.RandomSelect, d, client.DefaultOption)
	defer xclient.Close()

	args := &example.Args{
		A: 10,
		B: 20,
	}

	reply := &example.Reply{}
	err := xclient.Call(context.Background(), "Mul", args, reply)
	if err != nil {
		log.Fatalf("failed to call: %v", err)
	}

	log.Printf("%d * %d = %d", args.A, args.B, reply.C)

}

```
{% endtab %}
{% endtabs %}

执行`go run server/main.go`启动rpcx服务

服务端输出

```text
2019/06/25 19:38:45 server.go:174: ?[32mINFO ?[0m: server pid:15216
```

开启一个新的控制台，执行`go run client/main.go`

客户端输出

```text
2019/06/25 19:39:04 10 * 20 = 200
```

服务端输出

```text
2019/06/25 19:39:04 server.go:357: ?[32mINFO ?[0m: client has closed this connection: 127.0.0.1:52028
```





