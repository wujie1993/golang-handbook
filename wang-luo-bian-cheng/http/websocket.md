# Websocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

开源项目[github.com/gorilla/websocket](https://github.com/gorilla/websocket)实现了go语言层面的websocket协议

下载代码库

```text
go get github.com/gorilla/websocket
```

例子：通过Websocket实现双向通讯

{% code-tabs %}
{% code-tabs-item title="server.go" %}
```go
// Copyright 2015 The Gorilla WebSocket Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.

// +build ignore

package main

import (
	"flag"
	"html/template"
	"log"
	"net/http"

	"github.com/gorilla/websocket"
)

var addr = flag.String("addr", "localhost:8080", "http service address")

var upgrader = websocket.Upgrader{} // use default options

func echo(w http.ResponseWriter, r *http.Request) {
	c, err := upgrader.Upgrade(w, r, nil)
	if err != nil {
		log.Print("upgrade:", err)
		return
	}
	defer c.Close()
	for {
		mt, message, err := c.ReadMessage()
		if err != nil {
			log.Println("read:", err)
			break
		}
		log.Printf("recv: %s", message)
		err = c.WriteMessage(mt, message)
		if err != nil {
			log.Println("write:", err)
			break
		}
	}
}

func home(w http.ResponseWriter, r *http.Request) {
	homeTemplate.Execute(w, "ws://"+r.Host+"/echo")
}

func main() {
	flag.Parse()
	log.SetFlags(0)
	http.HandleFunc("/echo", echo)
	http.HandleFunc("/", home)
	log.Fatal(http.ListenAndServe(*addr, nil))
}

var homeTemplate = template.Must(template.New("").Parse(`
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script>  
window.addEventListener("load", function(evt) {

    var output = document.getElementById("output");
    var input = document.getElementById("input");
    var ws;

    var print = function(message) {
        var d = document.createElement("div");
        d.innerHTML = message;
        output.appendChild(d);
    };

    document.getElementById("open").onclick = function(evt) {
        if (ws) {
            return false;
        }
        ws = new WebSocket("{{.}}");
        ws.onopen = function(evt) {
            print("OPEN");
        }
        ws.onclose = function(evt) {
            print("CLOSE");
            ws = null;
        }
        ws.onmessage = function(evt) {
            print("RESPONSE: " + evt.data);
        }
        ws.onerror = function(evt) {
            print("ERROR: " + evt.data);
        }
        return false;
    };

    document.getElementById("send").onclick = function(evt) {
        if (!ws) {
            return false;
        }
        print("SEND: " + input.value);
        ws.send(input.value);
        return false;
    };

    document.getElementById("close").onclick = function(evt) {
        if (!ws) {
            return false;
        }
        ws.close();
        return false;
    };

});
</script>
</head>
<body>
<table>
<tr><td valign="top" width="50%">
<p>Click "Open" to create a connection to the server, 
"Send" to send a message to the server and "Close" to close the connection. 
You can change the message and send multiple times.
<p>
<form>
<button id="open">Open</button>
<button id="close">Close</button>
<p><input id="input" type="text" value="Hello world!">
<button id="send">Send</button>
</form>
</td><td valign="top" width="50%">
<div id="output"></div>
</td></tr></table>
</body>
</html>
`))
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% code-tabs %}
{% code-tabs-item title="client.go" %}
```go
// Copyright 2015 The Gorilla WebSocket Authors. All rights reserved.
// Use of this source code is governed by a BSD-style
// license that can be found in the LICENSE file.

// +build ignore

package main

import (
	"flag"
	"log"
	"net/url"
	"os"
	"os/signal"
	"time"

	"github.com/gorilla/websocket"
)

var addr = flag.String("addr", "localhost:8080", "http service address")

func main() {
	flag.Parse()
	log.SetFlags(0)

	interrupt := make(chan os.Signal, 1)
	signal.Notify(interrupt, os.Interrupt)

	u := url.URL{Scheme: "ws", Host: *addr, Path: "/echo"}
	log.Printf("connecting to %s", u.String())

	c, _, err := websocket.DefaultDialer.Dial(u.String(), nil)
	if err != nil {
		log.Fatal("dial:", err)
	}
	defer c.Close()

	done := make(chan struct{})

	go func() {
		defer close(done)
		for {
			_, message, err := c.ReadMessage()
			if err != nil {
				log.Println("read:", err)
				return
			}
			log.Printf("recv: %s", message)
		}
	}()

	ticker := time.NewTicker(time.Second)
	defer ticker.Stop()

	for {
		select {
		case <-done:
			return
		case t := <-ticker.C:
			err := c.WriteMessage(websocket.TextMessage, []byte(t.String()))
			if err != nil {
				log.Println("write:", err)
				return
			}
		case <-interrupt:
			log.Println("interrupt")

			// Cleanly close the connection by sending a close message and then
			// waiting (with timeout) for the server to close the connection.
			err := c.WriteMessage(websocket.CloseMessage, websocket.FormatCloseMessage(websocket.CloseNormalClosure, ""))
			if err != nil {
				log.Println("write close:", err)
				return
			}
			select {
			case <-done:
			case <-time.After(time.Second):
			}
			return
		}
	}
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}

开启一个控制台，运行`go run server.go`启动http服务

再开启一个新的控制台，运行`go run client.go`运行http客户端

这时客户端会开始往服务端发送时间戳，而服务端接收到时间戳后也会将时间戳返回给客户端，两边控制台中不断地输出时间戳，整个传输过程是在一个tcp连接中完成的。

服务端输出

```text
recv: 2019-06-24 22:15:34.7045514 +0800 CST m=+1.318580901
recv: 2019-06-24 22:15:35.7040721 +0800 CST m=+2.318101601
recv: 2019-06-24 22:15:36.7053066 +0800 CST m=+3.319336101
recv: 2019-06-24 22:15:37.7043016 +0800 CST m=+4.318331101
recv: 2019-06-24 22:15:38.7047991 +0800 CST m=+5.318828601
recv: 2019-06-24 22:15:39.7049525 +0800 CST m=+6.318982001
```

客户端输出

```text
connecting to ws://localhost:8080/echo
recv: 2019-06-24 22:15:34.7045514 +0800 CST m=+1.318580901
recv: 2019-06-24 22:15:35.7040721 +0800 CST m=+2.318101601
recv: 2019-06-24 22:15:36.7053066 +0800 CST m=+3.319336101
recv: 2019-06-24 22:15:37.7043016 +0800 CST m=+4.318331101
recv: 2019-06-24 22:15:38.7047991 +0800 CST m=+5.318828601
recv: 2019-06-24 22:15:39.7049525 +0800 CST m=+6.318982001
```

再打开浏览器访问localhost:8080，点击其中的Open按钮，开启一个websocket连接，再点击Send按钮会发送文本框中的内容，服务端接收到会输出在控制台，并返回给客户端

服务端输出

```text
recv: Hello world!
```

浏览器输出

![](../../.gitbook/assets/image%20%285%29.png)

浏览器端和客户端连接的是同一个HandlerFunc，即/echo路径所注入的echo\(w http.ResponseWriter, r \*http.Request\)方法，在方法体中通过websocket.Upgrader对象的Upgrade\(\)方法将连接升级为websocket协议的连接，通过调用websocket连接的ReadMessage\(\)方法，从客户端读取一个消息。当没有消息过来时，会处于阻塞状态；当连接断开时，则会引发错误。将响应的结果通过websocket连接的WriteMessage\(\)方法进行返回。

在客户端上通过websocket.DefaultDialer.Dial\(\)方法连接websocket服务端，提供连接地址和请求路径。连接成功后也会返回一个与服务端一致的websocket连接，同样具有ReadMessage\(\)方法和WriteMessage\(\)方法可用于读取和发送数据。以此实现了服务端与客户端的双向通讯。

，

