# Q&A

Q: 如何避免http服务下载csv文件时出现中文乱码

{% embed url="https://blog.wangjunfeng.com/post/golang-csv/" %}

Q: goroutine运行报错fatal error: newproc: function arguments too large for new goroutine

每个gouroutine中加载的对象大小是有限制的，使用值传递的方式在goroutine中赋值时，容易出现上述报错，需要将goroutine中使用的参数传递方式改为指针（特别是对于结构庞大的对象），避免发生对象的内存复制

