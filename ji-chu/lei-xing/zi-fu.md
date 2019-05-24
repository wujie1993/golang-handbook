# 字符

支持2种字符类型：`byte`和`rune`

| 类型 | 字节长度 | 编码 |
| :--- | :--- | :--- |
| byte | 1 | UTF-8 |
| rune | 4 | Unicode |

从源码角度看，`byte`等同于`uint8`，`rune`等同于`uint32`

例子：定义字符

```go
var c1 byte = 'a'
var c2 = 'b'
c3 := 'c'

var c4 rune = '嘿'
var c5 = '你'
c6 := '好'
```

