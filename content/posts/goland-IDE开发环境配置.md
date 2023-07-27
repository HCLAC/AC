---
title: goland-IDE开发环境配置
date: 2020-10-20 15:09:57
description: mac笔记本下配置goland-IDE开发环境
tags:
    - golang
    - goland
    - 开发环境
---

&emsp;&emsp;goland由JetBrains公司提供的golang语言集成开发工具，基于IntelliJ平台，功能强大，插件丰富，易上手理解。

> 下载地址: https://www.jetbrains.com/go/

本机环境：
> macos–10.15.5 (19F101)  
> goland–2020.1  
> golang–1.12.7(brew下载)  
> git代码管理  

goland插件：
> 默认插件、git/github .ignore、markdown、sftp等  

### 项目创建
因为现在很多开发都是结合git来维护代码，那么一般有两种方式获取golang项目  
1. 创建项目：`file->new->project`，项目目录最好选择在GOPATH目录下的src里面，如果有自己的github仓库，选择在`github.com/xxx/`下，这样不需要配置项目，即可使用github上的第三方库，这种情况适用于vendor方式管理依赖库。
2. 从github或者gitlab一些代码管理服务上拉取golang项目。 
    - 使用go get，会下载到GOPATH目录下的src里，常见参数：  
        -v显示执行命令日志。  
        -d 下载完成后编译安装  
        -u 同步下载依赖库  
    - 使用git clone缺省模式会下载到当前目录，当然git clone也可以指定存放目录:  
        `git clone https://github.com/xxx/xxx.git xxx`。

### 项目配置
- 新项目，配置`.gitignore`，如果安装.ignore插件，右键项目在`new->.ignore file`里面选择`.gitignore`，可以选择模板，比如用户模板、语言模板 平台模板等。
- 配置gopath，goland有三种gopath: `Global/Project/Modules`，  
    - `Global`一般保持不变，如果使用vendor维护依赖库，其他不需要配置。
    - `Project`看项目位置，不在Global目录下，修改成项目位置。
    - `Modules`最好设置成Global目录，这样所有的mod下载包同一位置。
- 配置go mod，golang1.11.1官方提供的包管理工具。在goland的`Preferences->Go->Go Modules`里，勾选Enable，国内环境因素，最好配置环境变量：`GOPROXY=xxx`，方便下载一些包：
```
    https://goproxy.cn
    https://goproxy.io
    https://mirrors.aliyun.com/goproxy/
    http://127.0.0.1:1081   # 自己的翻墙工具
```
- 配置filewatcher，在`Preferences->tools->File Watchers`，常用工具如下：  
    - `go fmt` : 统一的代码格式化工具（必须）。
    - `golangci-lint` : 静态代码质量检测工具，用于包的质量分析（推荐）。运行提示找不到当前包的路径，可以修改`golangci-lint`里的环境变量$GOPATH，添加当前包路径。
    - `goimports` : 自动import依赖包工具（可选）。
    - `golint` : 代码规范检测，并且也检测单文件的代码质量，比较出名的Go质量评估站点Go Report在使用（可选）。  
    `go fmt/goimports/golangci-lint`是goland默认带的  
    golint可以自行下载`go get github.com/golang/lint`，复制go fmt配置，修改Name, Program, Arguments三项配置，其中Arguments需要加上-set_exit_status参数。

完成上述准备后，开启您的golang之旅吧~