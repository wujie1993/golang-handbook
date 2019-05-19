# 安装Golang

## 下载地址

{% embed url="https://golang.org/dl/" %}

## 安装配置

以安装golang-1.12.5-linux-amd64版为例：

```
[root@localhost ~]# wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz
[root@localhost ~]# tar -C /usr/local -xzf go1.12.5.linux-amd64.tar.gz
```

打开`/etc/profile`配置环境变量

```text
export PATH=$PATH:/usr/local/go/bin:/root/go/bin
export PATH=$GOPATH:/root/go
```

让环境变量配置生效

```text
[root@localhost ~]# source /etc/profile
```

完成安装配置后使用以下命令进行验证

```text
# 查看golang版本
go version

# 查看golang环境变量配置
go env
```



