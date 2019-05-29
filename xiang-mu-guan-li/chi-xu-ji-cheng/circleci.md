# CircleCI

## 官网地址

{% embed url="https://circleci.com/" %}

## 流水线

首先在代码仓库中创建`.circleci`目录，并在其中创建流水线配置文件`config.yml`。文件内容如下：

{% code-tabs %}
{% code-tabs-item title=".circleci/config.yml" %}
```yaml
# Golang CircleCI 2.0 configuration file
#
# Check https://circleci.com/docs/2.0/language-go/ for more details
version: 2
jobs:
  build:
    docker:
      - image: circleci/golang:1.12
    steps:
      - checkout
      - run: go build
```
{% endcode-tabs-item %}
{% endcode-tabs %}

将更新提交到代码仓库中，在配置完代码仓库账号绑定后，在CircleCI管理控制台左侧菜单点击`Add Projects`添加项目，选择要开启持续集成的仓库点击右侧按钮`Set Up Project`，在新页面中选择操作系统`Linux`和语言`Go`，点击下方`Start Building`。



