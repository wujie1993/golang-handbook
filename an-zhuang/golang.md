# 安装Golang

## 下载地址

{% embed url="https://golang.org/dl/" %}

## 安装配置

以安装golang-1.12.5-linux-amd64版为例：

```bash
wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.12.5.linux-amd64.tar.gz
```

打开`/etc/profile`配置环境变量

{% code-tabs %}
{% code-tabs-item title="/etc/profile" %}
```bash
export PATH=$PATH:/usr/local/go/bin:/root/go/bin
export GOPATH=/root/go
```
{% endcode-tabs-item %}
{% endcode-tabs %}

让环境变量配置生效

```bash
source /etc/profile
```

完成安装配置后使用以下命令进行验证

```text
# 查看golang版本
go version

# 查看golang环境变量配置
go env
```



