# Vim

## 插件清单

* nerdtree。文件目录树形插件
* nerdtree-git-plugin。nerdtree git插件
* vim-go。golang插件
* tagbar。标签插件，显示文件中的缩略信息
* molokai。molokai颜色主题插件

## 快速配置

{% embed url="https://github.com/wujie1993/dev-env/tree/master/vim" %}

下载配置脚本

```
git clone git@github.com:wujie1993/dev-env.git $GOPATH/src/github.com/wujie1993/dev-env
```

执行配置脚本

```text
cd $GOPATH/src/github.com/wujie1993/dev-env/vim && sh ./setup.sh
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



