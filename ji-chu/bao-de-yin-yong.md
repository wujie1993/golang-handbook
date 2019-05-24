# 包

## 概念

将项目根据代码的逻辑或结构进行拆分，形成包，关键字`package`。通常以目录为单位定义包，包名与目录名保持一致

例子：定义一个包

```go
// 定义一个包a
package a
```

## 引用包

当一个包中需要用到另一个包中的内容时，可以对另一个包进行引用，关键字为`import`。进行包引用时需要指定被引用的包的路径，先是在`$GOROOT/src`路径下寻找，再到`$GOPATH/src`路径下寻找，最后到当前项目中的`vendor`路径下寻找

例子：在包a中引用包b

```go
// 定义一个包a
package a

// 引用包b
import "b"

// 定义一个A方法，调用包b中的B方法
func A(){
    b.B()
}
```

当引用一个或多个包时，也可以用括号包起来

例子：在包a中引用多个包

```go
// 定义一个包a
package a

// 引用包b和包c
import (
    "b"
    "c"
)

// 定义一个A方法，调用包b中的B方法和包c中的C方法
func A(){
    b.B()
    c.C()
}
```

### 别名引用

当包名出现重复时，可以为包定义一个别名，为包名定义别名后对于该引用包的调用全都改用别名

例子：为引用包定义别名

```go
// 定义一个包A
package a

// 为引用包c/a定义一个别名pkgA
import (
     pkgA "c/a"
)

// 定义一个A方法，调用别名为pkgA的包c/a中的方法Test()
func A(){
    pkgA.Test()
}
```

### 匿名引用

当包名出现重复时，除了为引用的包定义别名外，还可以使用关键字`.`,对于引用包的调用无需再指定包名

例子：不指定引用包名

```go
// 定义一个包A
package a

// 引用包c/a，并忽略其包名
import (
     . "c/a"
)

// 定义一个A方法，调用包c/a中的方法Test()
func A(){
    Test()
}
```

### 包初始化

在每个包中都可以定义一个`init()`方法，该方法通常用于做一些初始化工作。对于包进行引用时，首先会执行引用包中的`init()`方法。

例子：包的初始化

```go
// 定义包a
package a

import (
    "fmt"
)

// 定义包初始化方法，在别的包引用到包a时，会首先执行该方法
func init() {
        fmt.Println("Initialize the package here")
}
```

### 初始化引用

如果只是需要执行引用包中的`init`方法，没有调用到其他内容时，可以使用关键字`_`

```go
// 定义一个包A
package a

// 引用包c/a,只调用到其中的init()方法
import (
     _ "c/a"
)
```

{% hint style="info" %}
在引用包后必须调用到引用包中的内容\(使用了`_`关键字除外\)，否则会编译出错
{% endhint %}

{% hint style="info" %}
使用关键字`.`引用包时，需要注意两个包中定义的内容没有重名
{% endhint %}

{% hint style="info" %}
包的引用不能出现循环引用，以下用法是错误的：

1. 包a引用了包b，包b引用包a
2. 包a引用了包b，包b引用了包c，包c引用包a
{% endhint %}

## 可访问性

只有首字母为大写的内容才可以被别的包调用到

例子：可被其他包调用的内容

```go
var Age int
const Name = 'Mike'
func Public() {
}
```

例子：不可被其他包调用的内容

```go
var age int
const name = 'mike'
func private() {
}
```

