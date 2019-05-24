# Hello world

一个标准的`golang`项目主要分为两种：程序和包

## 程序

程序项目可编译出二进制可执行程序，必须以`package main`定义一个程序入口包，并在该包中定义一个`func main()`方法，作为程序的入口方法。程序执行时会从main方法开始执行，在main方法执行完成后，程序的进程也会随着结束

例子：Hello,world!

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

{% embed url="https://play.golang.org/p/I6Kx7guxUmm" caption="在线例子：Hello,world!" %}

在`$GOPATH/src`路径下创建目录`helloworld`，并在该目录中创建上述文件`main.go`，执行以下命令执行代码：

```bash
// go run {文件名} 指定入口方法所在的文件路径
go run main.go
```

或者对代码进行编译，生成二进制可执行文件，通过二进制文件执行

```bash
// 编译代码，编译时会在当前目录中寻找package main和其中的func main,编译后会默认生成一个与当前目录同名的二进制可执行文件，也可以通过-o参数指定输出路径
go build
// 执行程序
./helloworld
```

## 



