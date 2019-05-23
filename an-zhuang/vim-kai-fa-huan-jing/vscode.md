# VSCode

## 下载地址

{% embed url="https://code.visualstudio.com/download" %}

## 推荐插件

* Go。Go语言插件
* SFTP。用于将代码通过sftp快速同步到服务器端，通过服务器端进行编译构建
* Markdown All in One。Markdown编写快捷键，自动生成内容目录
* YAML。yaml语法支持，内置kubernetes yaml文件语法联想支持
* Docker。dockerfile语法高亮

{% hint style="info" %}
安装go语言插件的时候需要翻墙，否则会有部分包无法下载。
{% endhint %}

在无法翻墙的情况下，可以到github上搜索到无法下载的包的镜像同步项目，将项目下载到对应的本地路径上，如：

```bash
git clone git@github.com:golang/tools.git $GOPATH/src/golang.org/x/tools
```

