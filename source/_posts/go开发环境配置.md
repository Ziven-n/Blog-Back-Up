---
title: go开发环境配置
tags:
  - 环境配置
categories:
  - go
date: 2025-05-23 11:19:22
---

> **********安装**********
<!-- more -->



####  通过Homebrew安装
```
brew install go
```

####  验证版本号
```
go version
```

----

> **********配置环境变量**********

####  打开```.zshrc```文件
```
open ~/.zshrc
```
####  添加以下内容
```
export GOPATH=$HOME/go
export GOROOT="$(brew --prefix golang)/libexec"
export PATH=$PATH:$GOROOT/bin:$GOPATH/bin

# 设置 Go 代理（国内建议使用）
export GO111MODULE=on
export GOPROXY=https://goproxy.cn,direct
```


####  保存配置
```
source ~/.zshrc
```

####  检查环境
```
go env
```
#### go路径不对时可以使用以下命令查看
```
brew info go
```
#### 打开```vscode``` 安装go插件
安装完成后打开命令面板
搜索```go install/update Tools```点击后全选

创建新项目文件夹
比如```testapi```

在命令行进入此文件夹
执行
```
go mod init testapi
```

至此可以创建.go文件开始编码
