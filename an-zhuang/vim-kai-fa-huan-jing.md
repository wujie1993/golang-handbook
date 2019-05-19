# Vim开发环境

## 快速配置

{% embed url="https://github.com/wujie1993/dev-env/tree/master/vim" %}

下载配置脚本

```
[root@localhost ~]# git clone git@github.com:wujie1993/dev-env.git $GOPATH/github.com/wujie1993/dev-env
```

执行配置脚本

```text
[root@localhost ~]# cd $GOPATH/github.com/wujie1993/dev-env/vim
[root@localhost vim]# sh ./setup.sh
```

打开vim编辑器

安装vim插件

```text
:PluginInstall
```

安装golang插件

```text
:GoInstallBinaries
```

重新打开vim编辑器



