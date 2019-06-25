# 框架对比

本次统计时间截止至2019/6/25

## 项目数据统计

| 框架名称 | Watch | Starts | Forks | Issues Open | Issues Close | 最新版本 | 最近提交 | 开源协议 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Beego | 1257 | 20937 | 4252 | 619 | 1865 | v1.10.0 | 2019/6/18 | Apache 2.0 |
| Echo | 535 | 14266 | 1293 | 17 | 841 | v4.1.6 | 2019/6/21 | MIT |
| Gin | 1109 | 28489 | 3287 | 147 | 1017 | v1.4.0 | 2019/6/18 | MIT |
| Iris | 668 | 15143 | 1587 | 51 | 678 | v11.1.1 | 2019/6/23 | BSD-3 |
| Revel | 568 | 11150 | 1339 | 69 | 820 | v0.21.0 | 2018/10/30 | MIT |

可以看出在关注度上Gin是最高的，Beego次之，Echo与Iris相差不大，Revel最低

而在未解决的问题数量上Beego是最多的，Gin次之，Echo，Iris和Revel较少

在代码提交活跃度上，Beego，Echo，Gin和Iris都在近期维持着更新，Revel有半年以上未更新代码

## 成熟度对比

| 框架名称 | 代码质量 | 覆盖率 | 中英文档 | QQ群 | 论坛 | Gitter | Slack |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Beego | A+ |  | ✔ | ✔ | ✔ |  | ✔ |
| Echo | A+ | 84% | ✔ |  | ✔ | ✔ |  |
| Gin | A+ | 98% | ✔ |  |  | ✔ |  |
| Iris | A+ |  | ✔ |  | ✔ |  |  |
| Revel | A |  | ✔ |  |  |  |  |

Gin在代码覆盖率上有着极佳的表现，Echo也是达到了优秀水准，其他几个框架未提供代码覆盖率报告

在交流与反馈便利性上，Beego是做得最好的

## 功能与特性对比

| 功能名称 | Beego | Echo | Gin | Iris | Revel |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 路由器：命名路径参数和通配符 | ✔ | ✔ | ✔ | ✔ | ✔ |
| 路由器：正则表达式 | ✔ |  |  | ✔ |  |
| 路由器：分组 | ✔ | ✔ | ✔ | ✔ |  |
| 路由器：以上所有混合无冲突 |  |  |  | ✔ |  |
| 路由器：自定义HTTP错误 | ✔ | ✔ |  | ✔ | ✔ |
| 与`net/http`完全兼容 |  |  | ✔ | ✔ |  |
| 中间件生态 | ✔ | ✔ | ✔ | ✔ |  |
|  `Sinatra`风格API | ✔ | ✔ | ✔ | ✔ |  |
| 服务：自动`HTTPS` |  | ✔ |  | ✔ |  |
| 服务：优雅关闭 | ✔ |  |  | ✔ | ✔ |
| 服务：多监听 |  |  |  | ✔ |  |
| 完整的`HTTP/2` |  | ✔ |  | ✔ |  |
| 子域名绑定 |  |  |  | ✔ |  |
| Session管理 | ✔ |  |  | ✔ | ✔ |
| Websockets | ✔ |  |  | ✔ | ✔ |
| 视图模板编译到二进制文件中 |  |  |  | ✔ |  |
| 视图引擎：标准`html/template` | ✔ | ✔ | ✔ | ✔ | ✔ |
| 视图引擎：Pug |  |  |  | ✔ |  |
| 视图引擎：Django |  |  |  | ✔ |  |
| 视图引擎：Handlebars |  |  |  | ✔ |  |
| 视图引擎：Amber |  |  |  | ✔ |  |
| 响应渲染（Markdown，JSON，JSONP，XML） | ✔ | ✔ | ✔ | ✔ | ✔ |
| MVC模式 | ✔ |  |  | ✔ |  |
| 缓存 | ✔ |  |  | ✔ | ✔ |
| 文件服务器 | ✔ | ✔ | ✔ | ✔ | ✔ |
| 静态文件编译到二进制文件中 |  |  |  | ✔ |  |
| 响应可以在发送之前的生命周期中多次修改 |  |  |  | ✔ |  |
| Gzip压缩 | ✔ |  |  | ✔ | ✔ |
| 测试框架 |  |  |  | ✔ |  |
| Typescript代码转换器 |  |  |  | ✔ |  |
| 在线编辑 |  |  |  | ✔ |  |
| 日志系统 | ✔ | ✔ | ✔ | ✔ | ✔ |
| 维护和自动更新 |  |  |  | ✔ |  |
| 统计（功能点实现数） | 16 | 11 | 9 | 33 | 11 |

可以看到Iris的功能支持是最多的，Beego次之，Echo和Revel相同，Gin最少。

## 总结

从成熟度上说Gin和Beego属于第一梯队，Echo中规中矩，Iris近期发展势头旺盛，Revel逐渐退出舞台。

在性能方面Gin，Echo，Iris都是比较高的，Beego中规中矩，Revel性能一般。

在上手难度上Gin和Echo比较轻量容易学，Beego和Iris功能模块较多稍微庞大一些，Revel的设计理念有些许不同，难度较高。

## 词汇解析

**路由器：命名路径参数和通配符**

可以将处理程序注册到具有动态路径的路由。

示例：路径名称匹配

路径参数`"/user/{username}"`能够匹配到`"/user/me"`和`"/user/speedwheel"`

路径参数`username`匹配值分别是 `me` 和 `speedwheel`。

示例：路径通配符匹配

路径参数`"/user/{path *wildcard}"`能够匹配到`"/user/some/path/here"`和`"/user/this/is/a/dynamic/multi/level/path"`

`path` 路径参数的值分别是`some/path/here`和`this/is/a/dynamic/multi/level/path`。

> Iris也支持一种称为宏的功能，可以描述为/user/{username:string}或/user/{username:int min\(1\)}

**路由器：正则表达式**

可以使用带有过滤器的动态路径向具有过滤器的路径注册处理程序时，应该传递一些处理程序以执行处理程序。

示例：`"/user/{id ^[0-9]$}" matches to "/user/42" but not to "/user/somestring"`

> id路径参数的值为42。

**路由：分组**

可以将公共逻辑或中间件处理程序注册到共享相同路径前缀的特定路由组。

示例：对路由路径进行分组

```go
myGroup := Group("/user", userAuthenticationMiddleware)
myGroup.Handle("GET", "/", userHandler)
myGroup.Handle("GET","/profile", userProfileHandler)
myGroup.Handle("GET", "/signup", getUserSignupForm)
```

得到以下路径

* /user
* /user/profile
* /user/signup

您甚至可以从组中创建子组：

```go
myGroup.Group("/messages", optionalUserMessagesMiddleware)
myGroup.Handle("GET', "/{id}", getMessageByID)
```

得到以下路径

* /user/messages/{id}

**路由：以上所有混合无冲突**

这是一个先进但有用的功能，我们许多人希望它由路由器或Web框架支持，目前只有Iris在Go世界中支持这一功能。

这意味着`/{path *wildcard}`和`/user/{username}`和`/user/static`和`/user/{path*wildcard}`之类的东西可以在同一个路由器中注册，它可以正确匹配而不会受到静态路径的冲突（/user/static）或通配符（/{path\*wildcard}）。

**路由：自定义HTTP错误**

可以为错误状态码注册处理程序，状态代码&gt; = 400时表示错误状态代码。

示例：`OnErrorCode(404, myNotFoundHandler)`，表示当出现404状态码时，交由myNotFoundHander处理

上面的大多数Web框架仅支持404,405和500注册，但像Iris，Beego和Revel这样的功能完全支持任何状态代码甚至任何错误代码。

**完全兼容net/http**

意味着：

* 框架为您提供了直接访问`*http.Request`和`http.ResponseWriter`的上下文。
* 一种将`net/http`处理程序转换为特定框架的`Handler`类型的方法。

**中间件生态系统**

不必自己用中间件包装每个处理程序时，框架会为您提供一个完整的引擎来定义流，全局或每个路由或每组路由。

**类似 Sinatra API**

以不同的请求方法请求同意路径时，可以交由不同的处理方法处理。

示例：对于/path路径的Get请求交由getHander方法处理，对于/path路径的Post请求教育postHander方法处理，以此类推

**服务：自动HTTPS**

框架服务支持注册和自动续订`SSL`认证以及管理`SSL/TLS`传入连接（https）。

**服务：优雅关闭**

按CTRL + C关闭终端应用程序时; 服务器将正常关闭，等待一些连接完成其工作（具有特定的超时）或触发自定义事件以进行清理（即数据库关闭）。

**服务：多监听**

当框架的服务器支持注册自定义`net.Listener`或使用多个http服务器和地址提供Web应用程序时。

**完整的HTTP/2**

框架支持带有`https`的`HTTP/2`和服务器推送功能。

**子域名绑定**

当您可以直接从Web应用程序注册每个x，y子域的路由。

这个框架不支持这个功能，但你仍然可以通过启动多个http服务器来实现它，这样做的缺点是主应用程序和子域没有连接，默认情况下不可能在它们之间共享逻辑。

**Session会话**

* 支持http会话并准备在特定处理程序中使用时。
* 一些Web框架支持后端数据库来存储会话，因此您可以在服务器重新启动之间获得持久性。Buffalo使用gorilla会话，这些会话比其他实现慢一点。

示例：`func setValue(context http_context){ s := Sessions.New(http_context) s.Set("key", "my value") } func getValue(context http_context){ s := Sessions.New(http_context) myValue := s.Get("key") } func logoutHandler(context http_context){ Sessions.Destroy(http_context) }`

Wiki: [https://en.wikipedia.org/wiki/Hypertext\_Transfer\_Protocol\#HTTP\_session](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#HTTP_session)

**套接字\(WebSockets\)**

当框架支持websocket通信协议时。 实现是不同的。

您应该搜索他们的示例以查看适合您的内容。 我尝试所有这些的同事告诉我，与其他API相比，Iris使用更简单的API实现了最具特色的`webosocket`连接。

Wiki: [https://en.wikipedia.org/wiki/WebSocket](https://en.wikipedia.org/wiki/WebSocket)

**视图模板编译到二进制文件中**

通常，模板文件与Web应用程序的可执行文件是分开存放的。 编译到二进制文件中意味着框架支持与`go-bindata`集成，因此最终的可执行文件包含其中的模板，表示为`[]byte`。

什么是视图引擎？

框架支持模板加载，自定义和构建模板功能时，可以在关键部件上完成。

**视图引擎：STD**

框架支持通过标准`html/template`解析器加载模板。

**视图引擎：Pug**

框架支持通过`Pug`解析器加载模板。

**视图引擎：Django**

框架支持通过`Django`解析器加载模板。

**视图引擎：Handlebars**

当框架支持通过`Handlebars`解析器加载模板时。

**视图引擎：Amber**

当框架支持通过`Amber`解析器加载模板时。

**响应渲染：Markdown，JSON，JSONP，XML**

当框架的上下文为您提供一种简单的方法来轻松地发送/和自定义各种内容类型的响应。

**MVC模式**

模型 - 视图 - 控制器（MVC）是用于在计算机上实现用户界面的软件架构模式。  
它将给定的应用程序划分为三个相互关联的部分。  
这样做是为了将信息的内部表示与向用户呈现和接受信息的方式分开。  
MVC设计模式将这些主要组件分离，从而实现高效的代码重用和并行开发。

* Iris支持完整的MVC功能，可以在运行时注册。
* Beego仅支持方法和模型匹配，可以在运行时注册。
* Revel支持方法，路径和模型匹配，只能通过生成器（必须运行以构建Web应用程序的不同软件）注册。

Wiki: [https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)

**缓存**

Web缓存（或HTTP缓存）是用于临时存储（缓存）Web文档（例如HTML页面和图像）的信息技术，以减少服务器滞后。  
通过它的Web缓存系统文档; 如果满足某些条件，可以满足后续要求。\[1\] Web缓存系统可以指设备或计算机程序。

Wiki: [https://en.wikipedia.org/wiki/Web\_cache](https://en.wikipedia.org/wiki/Web_cache)

**文件服务器**

可以将（物理）目录注册到将自动向客户端提供此目录文件的路由。

**静态文件服务编译到二进制文件中**

通常，静态文件（如资产; css，javascript文件…）与应用程序的可执行文件是分开存放的。  
支持此功能的框架使您有机会将所有这些数据嵌入到应用程序中，表示为`[]byte`，它们的响应时间也更快，因为服务器可以直接为它们提供服务，而无需在物理位置查找文件。

**响应可以在发送之前的生命周期中多次修改**

目前只有Iris通过其http\_context中的内置响应编写器支持此功能。

当框架支持此功能时，您可以在发送到客户端之前检索或重置或修改写入的状态代码，正文和标题（在基于`net/http`的Web框架中，默认情况下这是不可能的，因为无法检索或更改正文和状态代码 书面）。

**Gzip**

当你在路由的处理程序中并且你可以更改响应编写器以便使用gzip压缩发送响应时，框架应该处理已发送的头文件，如果发生任何错误，它应该将响应写入恢复正常。  
它也应该能够检查客户端是否支持gzip。

> gzip是一种文件格式和用于文件压缩和解压缩的软件应用程序

Wiki: [https://en.wikipedia.org/wiki/Gzip](https://en.wikipedia.org/wiki/Gzip)

**测试框架**

当您可以使用特定的框架库测试HTTP时，它的工作就是帮助您轻松编写更好的测试。

示例（目前，只有Iris支持）:`func TestAPI(t *testing.T) { app := myIrisApp() tt := httptest.New(t, app) tt.GET("/admin").WithBasicAuth("name","pass").Expect(). Status(httptest.StatusOK).Body().Equal("welcome") }`

myIrisApp返回你想象中的Web应用程序，它有一个`/admin`的GET处理程序，受基本身份验证保护。

上面的简单测试检查/admin是否以状态OK响应，并且使用特定用户名和密码传递身份验证，并且其正文为`welcome`。

**Typescript代码转换器**

`Typescript`目标是成为ES6的超集，除了标准定义的所有新东西之外，还将添加一个静态类型系统。  
`Typescript`还有一个转换器，它将我们的`Typescript`代码（即ES6 +类型）转换为ES5或ES3 `javascript`代码，因此我们可以在今天的浏览器中使用它。

**在线编辑**

在线编辑器借助在线编辑器，您可以快速轻松地在线编译和运行代码。

**日志系统**

自定义日志记录系统通过提供诸如颜色编码，格式化，日志级别分离，不同日志记录后端等有用功能来扩展本机日志包行为。

**维护和自动更新**

以非侵入方式通知用户“即时”更新框架。

## 参考来源

{% embed url="https://www.cnblogs.com/desmond123/p/9821687.html" %}



