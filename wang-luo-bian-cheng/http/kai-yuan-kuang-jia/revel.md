# Revel

一个高生产率，全栈网络框架

## 官方网站

{% embed url="https://revel.github.io/" %}

## 项目地址

{% embed url="https://github.com/revel/revel" %}

## 快速开始

下载与安装

```bash
go get -u github.com/revel/cmd/revel
```

例子：简单的Revel服务

Revel项目是没有main包的，项目代码通过revel命令行工具创建

运行命令`revel new revelsimple`创建一个新项目到`$GOPATH/src/revelsimple`路径上

执行`revel run revelsimple`启动服务

服务端输出

```text
Revel executing: run a Revel application
WARN  15:31:36    run.go:150: No http.addr specified in the app.conf listening on localhost interface only. This will not allow external access to your application
?[32mINFO ?[0m 15:31:39    app     run.go:26: Running revel server
?[32mINFO ?[0m 15:31:39    app revel_hooks.go:32: Go to /@tests to run the tests.
Revel proxy is listening, point your browser to : 9000
Revel engine is listening on.. localhost:58517
```

开启一个新的控制台，执行`curl 127.0.0.1:9000`

客户端输出

```text
<!DOCTYPE html>

<html>
  <head>
    <title>Home</title>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" type="text/css" href="/public/css/bootstrap-3.3.6.min.css">
    <link rel="shortcut icon" type="image/png" href="/public/img/favicon.png">
    <script src="/public/js/jquery-2.2.4.min.js"></script>
    <script src="/public/js/bootstrap-3.3.6.min.js"></script>


  </head>
  <body>


<header class="jumbotron" style="background-color:#A9F16C">
  <div class="container">
    <div class="row">
      <h1>It works!</h1>
      <p></p>
    </div>
  </div>
</header>

<div class="container">
  <div class="row">
    <div class="span6">




    </div>
  </div>
</div>


    <style type="text/css">
        #sidebar {
                position: absolute;
                right: 0px;
                top:69px;
                max-width: 75%;
                z-index: 1000;
                background-color: #fee;
                border: thin solid grey;
                padding: 10px;
        }
        #toggleSidebar {
                position: absolute;
                right: 0px;
                top: 50px;
                background-color: #fee;
        }

</style>
<div id="sidebar" style="display:none;">
        <h4>Available pipelines</h4>
        <dl>

                <dt>DevMode</dt>
                <dd>true</dd>

                <dt>RunMode</dt>
                <dd>dev</dd>

                <dt>_controller</dt>
                <dd>{App 0xc000223620 Index 0xc0000c3540 0xc00009d5e0 App.Index 127.0.0.1 0xc000110840 0xc0000c3ae0 0xc0000672c0 {map[] map[]} map[] 0xc0002b36e0 map[] map[DevMode:true RunMode:dev _controller:0xc000201790 currentLocale: errors:map[] flash:map[] session:map[] title:Home] 0xc0003a9050 0xc00005dba0}</dd>

                <dt>currentLocale</dt>
                <dd></dd>

                <dt>errors</dt>
                <dd>map[]</dd>

                <dt>flash</dt>
                <dd>map[]</dd>

                <dt>session</dt>
                <dd>map[]</dd>

                <dt>title</dt>
                <dd>Home</dd>

        </dl>
        <h4>Flash</h4>
        <dl>

        </dl>

        <h4>Errors</h4>
        <dl>

        </dl>
</div>
<a id="toggleSidebar" href="#" class="toggles"><i class="glyphicon glyphicon-chevron-left"></i></a>

<script>
        $sidebar = 0;
        $('#toggleSidebar').click(function() {
                if ($sidebar === 1) {
                        $('#sidebar').hide();
                        $('#toggleSidebar i').addClass('glyphicon-chevron-left');
                        $('#toggleSidebar i').removeClass('glyphicon-chevron-right');
                        $sidebar = 0;
                }
                else {
                        $('#sidebar').show();
                        $('#toggleSidebar i').addClass('glyphicon-chevron-right');
                        $('#toggleSidebar i').removeClass('glyphicon-chevron-left');
                        $sidebar = 1;
                }

    return false;
        });
</script>


  </body>
</html>
```

服务端输出

```text
?[32mINFO ?[0m 15:34:52    app server_adapter_go.go:158: Request Stats                            ?[32mip?[0m=127.0.0.1 ?[32mmethod?[0m=GET ?[32maction?[0m=App.Index ?[32mstatus?[0m=200 ?[32mduration_seconds?[0m=0.0459954   ?[32mpath?[0m=/ ?[32mnamespace?[0m=App\\ ?[32mstart?[0m=2019/06/25 15:34:52 ?[32msection?[0m=requestlog
```

