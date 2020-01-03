# 条件编译

根据运行平台的不同编译特定的代码文件。有两种方式：1. 标签注释；2. 文件后缀

在进行go build时会检查当前系统所对应的标签，只对符合标签条件的代码进行编译，支持的标签参见`$GOOS`与`$GOARCH`

## 标签注释

在要进行条件编译的代码文件头部添加`// +build`注释，支持将多个标签组合，以空格表示或，以逗号表示与，以`!`表示非

如：`// +build linux,amd64 windows,386`

表示仅在amd64架构的linux系统或者386架构的windows平台上编译，以伪代码表示为`(linux AND amd64) OR (windows AND 386)`

也可以分多行表示与，如：

```text
// +build linux windows,cgo
// +build !amd64
```

表示`(linux AND !amd64) OR (windows AND cgo AND !amd64)`

## 文件后缀

在要进行条件编译的代码文件名添加后缀`_$GOOS`，`_$GOARCH`或`_$GOOS_$GOARCH`。如：

signal\_windows.go表示仅在windows平台编译

signal\_linux\_amd64表示仅在amd64架构的linux平台上编译

## Tips

1. 标签注释与文件后缀可结合使用，根据实际场景灵活应用
2. 不添加标签注释或者文件后缀与`_$GOOS`，`_$GOARCH`或`_$GOOS_$GOARCH`不匹配时认定为可在所有平台编译
3. 尽量使用文件后缀表示条件编译，便于代码阅读

