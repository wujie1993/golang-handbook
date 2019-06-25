# grpc-gateway

grpc服务的代理网关，将grpc服务接口转为restful接口对外提供服务。

## 项目地址

{% embed url="https://github.com/grpc-ecosystem/grpc-gateway" %}

## 下载与安装

首先完成[grpc安装](https://golang-2.gitbook.io/handbook/wang-luo-bian-cheng/rpc/grpc#xia-zai-yu-an-zhuang)

接下来安装相关的包

```bash
go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway
go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger
go get -u github.com/golang/protobuf/protoc-gen-go
```

## 例子：简单的grpc-gateway服务

{% code-tabs %}
{% code-tabs-item title="main.go" %}
```go
package main

import (
	"context" // Use "golang.org/x/net/context" for Golang version <= 1.6
	"flag"
	"net/http"

	"github.com/golang/glog"
	"github.com/grpc-ecosystem/grpc-gateway/runtime"
	"google.golang.org/grpc"

	gw "demo/http/frameworks/grpc-gateway/simple/protos" // Update
)

var (
	// command-line options:
	// gRPC server endpoint
	grpcServerEndpoint = flag.String("grpc-server-endpoint", "localhost:9090", "gRPC server endpoint")
)

func run() error {
	ctx := context.Background()
	ctx, cancel := context.WithCancel(ctx)
	defer cancel()

	// Register gRPC server endpoint
	// Note: Make sure the gRPC server is running properly and accessible
	mux := runtime.NewServeMux()
	opts := []grpc.DialOption{grpc.WithInsecure()}
	err := gw.RegisterYourServiceHandlerFromEndpoint(ctx, mux, *grpcServerEndpoint, opts)
	if err != nil {
		return err
	}

	// Start HTTP server (and proxy calls to gRPC server endpoint)
	return http.ListenAndServe("localhost:8080", mux)
}

func main() {
	flag.Parse()
	defer glog.Flush()

	if err := run(); err != nil {
		glog.Fatal(err)
	}
}

```
{% endcode-tabs-item %}

{% code-tabs-item title="protos/helloworld.proto" %}
```text
syntax = "proto3";
package example;

import "google/api/annotations.proto";

message StringMessage {
  string value = 1;
}

service YourService {
  rpc Echo(StringMessage) returns (StringMessage) {
      option (google.api.http) = {
          post: "/v1/example/echo"
          body: "*"
      };
  }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

执行命令

```bash
protoc -I/usr/local/include -I. \
  -I$GOPATH/src \
  -I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
  --go_out=plugins=grpc:. \
  protos/helloworld.proto
```

这时protos/目录中会生成文件helloworld.pb.go，内容为grpc接口相关的定义

执行命令

```bash
protoc -I/usr/local/include -I. \
  -I$GOPATH/src \
  -I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
  --grpc-gateway_out=logtostderr=true:. \
  protos/helloworld.proto
```

这时protos/目录中会生成文件helloworld.pb.gw.go，内容为http代理接口相关的定义

执行命令

```text
protoc -I/usr/local/include -I. \
  -I$GOPATH/src \
  -I$GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
  --swagger_out=logtostderr=true:. \
  protos/helloworld.proto
```

这时protos/目录中会生成文件helloworld.swagger.json，内容为swagger api文档描述

执行命令`go run main.go`启动grpc代理网关服务

启动以下例子中的grpc服务端

{% embed url="https://golang-2.gitbook.io/handbook/wang-luo-bian-cheng/rpc/grpc\#jian-dan-grpc-fu-wu" %}

开启一个新的控制台，执行

```bash
curl -X POST "http://127.0.0.1:8080/v1/example/echo" -H "accept: application/json" -H "Content-Type: application/json" -d "{ \"value\": \"string\"}"
```

响应结果

```text
{"value":"Hello string"}
```



