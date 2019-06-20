# 性能测试

与单元测试相似，性能测试文件也是以\_test.go结尾，需要引入包testing。不同的是性能测试的方法定义格式为func BenchmarkXxx\(b \*testing.B\)，Xxx为大写开头的方法名。在测试方法中使用变量b.N做为测试的循环次数。

例子：序列化与反序列化性能测试

{% code-tabs %}
{% code-tabs-item title="b\_test.go" %}
```go
package unittest

import (
	"encoding/json"
	"testing"
	"time"
)

type Person struct {
	Name      string    `json:"name"`
	Age       int       `json:"age"`
	Gender    bool      `json:"gender"`
	Weight    float32   `json:"weight"`
	Height    float32   `json:"height"`
	Birthdate time.Time `json:"birthdate"`
}

func BenchmarkMarshalJson(b *testing.B) {
	me := Person{
		Name:   "viva",
		Age:    26,
		Gender: true,
	}
	for i := 0; i < b.N; i++ {
		json.Marshal(&me)
	}
}

func BenchmarkUnmarshalJson(b *testing.B) {
	for i := 0; i < b.N; i++ {
		json.Unmarshal([]byte(`{
		"name":   "viva",
		"age":    26,
		"gender": true,
	}`),
			new(Person))
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

使用`go test`命令默认不会执行性能测试方法，需要附加参数-test.bench，数值为正则表达式，用于指定执行方法名称匹配的测试方法。

执行测试命令

```bash
go test b_test.go -test.bench=".*"
```

测试结果：

```text
goos: linux
goarch: amd64
BenchmarkMarshalJson-2     	 1000000	      1791 ns/op
BenchmarkUnmarshalJson-2   	 2000000	       827 ns/op
PASS
ok  	command-line-arguments	4.432s
```

输出结果中第一列为测试案例名称，第二列为测试方法中循环次数（即b.N，该值大小由golang自行推断），第三列为此轮测试花费的时间。默认情况下只进行一轮测试，如果需要进行多轮的重复测试，可以附加参数-count={次数}。

执行测试命令

```bash
go test b_test.go -test.bench=".*" -count=5
```

测试结果：

```text
goos: linux
goarch: amd64
BenchmarkMarshalJson-2     	 1000000	      1536 ns/op
BenchmarkMarshalJson-2     	 1000000	      1603 ns/op
BenchmarkMarshalJson-2     	 1000000	      1661 ns/op
BenchmarkMarshalJson-2     	  500000	      2017 ns/op
BenchmarkMarshalJson-2     	  500000	      2389 ns/op
BenchmarkUnmarshalJson-2   	 2000000	       904 ns/op
BenchmarkUnmarshalJson-2   	 2000000	       814 ns/op
BenchmarkUnmarshalJson-2   	 2000000	       870 ns/op
BenchmarkUnmarshalJson-2   	 2000000	       920 ns/op
BenchmarkUnmarshalJson-2   	 2000000	       988 ns/op
PASS
ok  	command-line-arguments	21.624s
```

需要查看性能测试过程中的内存分配情况时，可以附加参数-benchmem

执行测试命令

```bash
go test b_test.go -test.bench=".*" -benchmem
```

测试结果：

```text
goos: linux
goarch: amd64
BenchmarkMarshalJson-2     	 1000000	      1619 ns/op	     240 B/op	       4 allocs/op
BenchmarkUnmarshalJson-2   	 2000000	       862 ns/op	     416 B/op	       9 allocs/op
PASS
ok  	command-line-arguments	5.198s
```

测试结果中多出的两列分别是每个测试循环占用的平均内存大小和内存分配次数



