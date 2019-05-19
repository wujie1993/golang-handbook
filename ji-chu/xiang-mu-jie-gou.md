# Hello world

一个标准的`golang`项目主要分为两种：包和程序

## 包

将项目根据代码的逻辑或结构进行拆分，形成包，关键字`package`。通常以目录为单位定义包，包名与目录名保持一致

例子：定义一个包

```go
// 定义一个包a
package a
```

## 程序

程序项目可编译出二进制可执行程序，必须以`package main`定义一个程序入口包，并在该包中定义一个`func main()`方法，作为程序的入口方法。程序执行时会从main方法开始执行，在main方法执行完成后，程序的进程也会随着结束

例子：Hello world程序

{% code-tabs %}
{% code-tabs-item title="main.go" %}
```go
// 定义包名main,表示该包为程序包
package main

// 引用内置包fmt,该包主要用于进行字符串格式化输出
import "fmt"

// 定义入口方法main,表示该方法为程序入口方法
func main(){
    // 调用fmt包中的Println方法,在控制台中输出Hello,world!字样
    fmt.Println("Hello,world!")
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

在`$GOPATH/src`路径下创建目录`helloworld`，并在该目录中创建上述文件`main.go`，执行以下命令运行程序：

```bash
// go run {文件名} 指定入口方法所在的文件路径
go run main.go
```

## 



