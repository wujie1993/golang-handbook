# 安装Golang

## 下载地址

{% embed url="https://golang.org/dl/" %}

## 安装配置

以安装go1.13.linux-amd64为例：

```bash
wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz
tar -C /usr/local -xzf go1.13.linux-amd64.tar.gz
```

编辑`/etc/profile`添加环境变量

{% code title="/etc/profile" %}
```bash
export PATH=$PATH:/usr/local/go/bin:/root/go/bin
export GOPATH=/root/go
```
{% endcode %}

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

## 准备开发环境

{% embed url="https://viva.gitbook.io/project/sui-bi/ide" %}

