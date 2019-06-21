# 单元测试

## 测试

在开发过程中，往往需要对某个方法或模块做功能测试和验证。在golang中内置了用于进行代码单元测试的包`testing`。

首先，对于需要测试的代码文件，在相同目录下创建一个同名带`_test.go`后缀的文件，比如代码文件a.go和单元测试文件a\_test.go。在单元测试文件中，引入包`testing`，在下方定义以需要验证的方法名加`Test`前缀，形式参数为\*testing.T的单元测试方法，比如需要测试方法B\(\)，则对应的单元测试方法为TestB\(\*testing.T\)，\*testing.T可以在测试过程中输出测试结果或者是报错中断测试。

例子：方法单元测试

{% code-tabs %}
{% code-tabs-item title="a.go" %}
```go
package unittest

func Add(x, y int) int {
	return x + y
}

func Sub(x, y int) int {
	return x - y
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="a\_test.go" %}
```go
package unittest

import (
	"testing"
)

func TestAdd(t *testing.T) {
	if Add(1, 2) != 3 {
		t.Error("error result of func A()")
	} else {
		t.Log("ok")
	}
}

func TestSub(t *testing.T) {
	if Sub(1, 2) != -1 {
		t.Error("error result of func Sub()")
	} else {
		t.Log("ok")
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

运行命令`go test -v`执行单元测试，`-v`参数表示打印测试过程中输出的日志（查看更多的参数请使用`go test -h`）

测试结果输出如下：

```text
=== RUN   TestAdd
--- PASS: TestAdd (0.00s)
    a_test.go:11: ok
=== RUN   TestSub
--- PASS: TestSub (0.00s)
    a_test.go:19: ok
PASS
ok  	_/root/Projects/demo/unittest	0.003s
```

如果当前目录中有多个单元测试文件可以使用`go test -v .`指定执行当前目录下的所有测试文件。

单元测试文件需要遵循以下规则：

* 文件名必须是`_test.go`结尾的，这样在执行`go test`的时候才会执行到相应的代码；
* 必须导入 `testing`包；
* 所有的测试用例函数必须是`Test`开头；
* 测试用例会按照代码的书写顺序依次执行；
* 测试函数`TestXxx()`的参数是`testing.T`，我们可以使用该类型来记录错误或者是测试状态；
* 测试方法格式：`func TestXxx (t *testing.T)`,`Xxx`部分可以为任意的字母数字的组合，但是首字母必须是大写；
* 函数中通过调用`testing.T`的`Error`, `Errorf`, `FailNow`, `Fatal`, `FatalIf`方法，说明测试不通过，调用`Log`方法用来记录测试的信息。

## 覆盖率

方法中往往带有许多的条件判断，在不同的测试条件下，测试的结果各不相同。为了保证测试代码的质量，需要尽可能多的覆盖测试方法中的各种条件，而代码的测试覆盖率可做为评估单元测试测试质量的一个依据。

在测试命令中附加参数-cover开启代码覆盖率统计

例子：代码覆盖率检查

创建目录$GOPATH/src/demo/coverage/，分别创建代码文件action.go和测试文件action\_test.go

{% code-tabs %}
{% code-tabs-item title="action.go" %}
```go
package coverage

import (
	"fmt"
)

func Action(kind string) {
	switch kind {
	case "cat":
		fmt.Println("喵")
	case "dog":
		fmt.Println("汪")
	case "frog":
		fmt.Println("呱")
	default:
		fmt.Println("I have no idea")
	}
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="action\_test.go" %}
```go
package coverage_test

import (
	"demo/coverage"
	"testing"
)

func TestAction(t *testing.T) {
	coverage.Action("cat")
	coverage.Action("frog")
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

执行命令

```bash
go test -v -cover
```

测试结果

```text
=== RUN   TestAction
喵
呱
--- PASS: TestAction (0.00s)
PASS
coverage: 60.0% of statements
ok  	demo/coverage	0.003s	coverage: 60.0% of statements
```

默认情况下只会检查\_test.go文件中引用到的包，通过附加参数-coverpkg {包名1},{包名2},...可以指定需要检查覆盖率的包。

覆盖率检查可以生成可视化的测试报告，首先在测试的时候附加参数-coverprofile=coverage.data，将代码覆盖报告输出到coverage.data中，再通过命令go tool cover -html=coverage.data -o coverage.html根据覆盖报告生成一个html文件

执行命令

```bash
go test -v -cover -coverprofile=coverage.data
go tool cover -html=coverage.data -o coverage.html
```

通过浏览器打开生成的html文件即可查看代码覆盖情况

![](../../.gitbook/assets/image%20%284%29.png)



