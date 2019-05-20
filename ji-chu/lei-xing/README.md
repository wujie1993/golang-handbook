# 类型

## 基本类型

### 整型

整型表示数字整数，共分为两类：有符号整型和无符号整型

#### 有符号整型

| 类型 | 长度（字节数） | 值范围 |
| :--- | :--- | :--- |
| int8 | 1 | -128 ~127 |
| int16 | 2 | -32768~32767 |
| int32 | 4 | -2147483648~2147483647 |
| int64 | 8 | -9223372036854775808~9223372036854775807 |
| int | 在32位平台为4，在64位平台为8 | 在32位平台等同于int32，在64位平台等同于int64 |

#### 无符号整型

| 类型 | 长度（字节数） | 值范围 |
| :--- | :--- | :--- |
| uint8 | 1 | 0~255 |
| uint16 | 2 | 0~65535 |
| uint32 | 4 | 0~4294967295 |
| uint64 | 8 | 0~18446744073709551615 |
| uint | 在32位平台为4，在64位平台为8 | 在32位平台等同于uint32，在64位平台等同于uint64 |
| uintptr | 同uint | 同uint |

例子：定义整型

```go
var age uint8
age = 18

var income int
income = -1000
```

### 浮点型

浮点型表示数字浮点数

* float32
* float64

例子： 定义浮点型

```go
var speed float32
speed = 35.75
```

### 复数型

复数型表示数字复数

* complex64
* complex128

例子：定义复数型

```go
var length1 complex64
length1 = 2.7 * 34i

var length2
// 使用内置方法complex(x,y)定义复数，x表示实部，y表示虚部
length2 = complex(2.7, 34)
```

### 布尔型

关键字`bool`，占用一个比特位，取值`true`和`false`

例子：定义布尔型

```go
var succeed bool
succeed = true
```

### 字符串

字符串类型关键字`string`

例子：定义字符串

```go
var name string
name = "mike"
```

{% hint style="info" %}
字符串类型只能整体重新赋值，无法修改其中的某一位字符
{% endhint %}





