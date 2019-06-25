# GRPC

## 官方网站

{% embed url="https://grpc.io/" %}

## 项目地址

{% embed url="https://github.com/grpc/grpc-go" %}

## 下载与安装

首先安装`protoc`

访问下方地址

{% embed url="https://github.com/protocolbuffers/protobuf/releases" %}

根据系统类型找到合适的安装包下载后解压，将其中的二进制文件移入`$PATH`目录中，将其中的`include/`目录下的文件复制到`/usr/local/lib/`目录下

接着下载grpc库

```bash
go get -u google.golang.org/grpc
```

如果无法访问外网则使用以下方式

```bash
git clone https://github.com/grpc/grpc-go.git $GOPATH/src/google.golang.org/grpc
git clone git@github.com:googleapis/go-genproto.git $GOPATH/src/google.golang.org/genproto
```

## 例子

### 简单grpc服务

{% code-tabs %}
{% code-tabs-item title="server/main.go" %}
```go
package main

import (
	"context"
	"log"
	"net"

	pb "demo/rpc/grpc/simple/protos"

	"google.golang.org/grpc"
)

const (
	port = "localhost:9090"
)

type server struct{}

func (s *server) Echo(ctx context.Context, in *pb.StringMessage) (*pb.StringMessage, error) {
	log.Printf("Received: %v", in.Value)
	return &pb.StringMessage{Value: "Hello " + in.Value}, nil
}

func main() {
	lis, err := net.Listen("tcp", port)
	if err != nil {
		log.Fatalf("failed to listen: %v", err)
	}
	s := grpc.NewServer()
	pb.RegisterYourServiceServer(s, &server{})
	if err := s.Serve(lis); err != nil {
		log.Fatalf("failed to serve: %v", err)
	}
}

```
{% endcode-tabs-item %}

{% code-tabs-item title="client/main.go" %}
```go
package main

import (
	"context"
	"log"
	"os"
	"time"

	pb "demo/rpc/grpc/simple/protos"

	"google.golang.org/grpc"
)

const (
	address     = "localhost:9090"
	defaultName = "world"
)

func main() {
	conn, err := grpc.Dial(address, grpc.WithInsecure())
	if err != nil {
		log.Fatalf("did not connect: %v", err)
	}
	defer conn.Close()
	c := pb.NewYourServiceClient(conn)

	name := defaultName
	if len(os.Args) > 1 {
		name = os.Args[1]
	}
	ctx, cancel := context.WithTimeout(context.Background(), time.Second)
	defer cancel()
	r, err := c.Echo(ctx, &pb.StringMessage{Value: name})
	if err != nil {
		log.Fatalf("could not greet: %v", err)
	}
	log.Printf("Greeting: %s", r.Value)
}

```
{% endcode-tabs-item %}

{% code-tabs-item title="protos/helloworld.proto" %}
```text
syntax = "proto3";
package example;

message StringMessage {
  string value = 1;
}

service YourService {
  rpc Echo(StringMessage) returns (StringMessage) {}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

执行命令

```text
protoc -I protos/ protos/helloworld.proto --go_out=plugins=grpc:protos
```

这时protos/目录下会生成文件helloworld.pb.go，该文件是grpc接口以及相关参数的定义

执行以下命令启动GRPC服务端

```bash
go run server/main.go
```

执行以下命令执行GRPC客户端

```text
go run client/main.go
```

服务端输出

```text
2019/06/25 17:32:09 Received: world
```

客户端输出

```text
2019/06/25 17:32:09 Greeting: Hello world
```

