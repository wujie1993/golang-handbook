# 字符

支持两种字符类型：`byte`和`rune`，使用符号`''`表示字符字面常量

| 类型 | 长度（byte） | 编码 |
| :--- | :--- | :--- |
| byte | 1（同uint8） | UTF-8 |
| rune | 4（同uint32） | Unicode |

例子：定义字符

```go
var c1 byte = 'a'
var c2 = 'b'
c3 := 'c'

var c4 rune = '嘿'
var c5 = '你'
c6 := '好'
```

