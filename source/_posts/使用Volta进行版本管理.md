---
title: 使用Volta进行版本管理
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2021-09-07 15:20:42
categories: 前端 #分类
top: 12
tags:
  - 前端开发
  - node yarn npm
---

### Volta 安装及使用

- 安装 [官方文档](https://docs.volta.sh/guide/getting-started)

```bash
curl https://get.volta.sh | bash
```

- 设置环境变量

```bash
export VOLTA_HOME="$HOME/.volta"
export PATH=$LOCAL/bin:$VOLTA_HOME/bin:$PATH
```

<!-- more -->

### Volta 命令介绍

volta默认的工作目录`VOLTA_HOME`位置如下

- ~/.volta on Unix
- %LOCALAPPDATA%\\Volta on Windows

因为后续下载安装的各种工具链都是存在该目录下的，所以我这里自定义`VOLTA_HOME`，比如更改为`/User/mark/.volta`目录

- 在环境变量中新建一个系统变量名为`VOLTA_HOME`，值设置`/User/mark/.volta`

因为更改了`VOLTA_HOME`,所以还需要配置下`Shim Directory`目录，否则通过volta install安装的packages的命令不能使用，比如安装hexo后无法使用hexo xxx命令
`Shim Directory`默认目录为`%VOLTA_HOME%\bin` (Unix下为`$VOLTA_HOME/bin`)

- 在环境变量中修改PATH中原来的VOLTA\_HOME部分

**注意**
修改环境变量后重新打开cmd使配置生效

### 安装指定版本Node

```bash
volta list //查看存在的版本
volta install node //安装最新版的nodejs
volta install node@12.2.0 //安装指定版本
volta install node@12 //volta将选择合适的版本安装
volta pin node@10.15 //将更新项目的package.json文件以使用工具的选定版本
volta pin yarn@1.14 //将更新项目的package.json文件以使用工具的选定版本
```

**技巧**
`volta install <package name>`安装tools时与网络有关系，有时会死活下载不下来（主要应该是国内网络环境的原因），可以将自己手动下载的压缩包，或者其他机器上已经使用volta安装过该工具所下载的压缩包（在`%VOLTA_HOME%\tools\inventory\`目录中），拷贝到`%VOLTA_HOME%\tools\inventory\`下对应的文件夹内，比如将`node-v12.18.2-win-x64.zip`复制到`%VOLTA_HOME%\tools\inventory\node\`目录下，然后再重新执行install命令

- 新选择一个目录，重新配置node\_global和node\_cache，配置npm config
- 卸载原来的版本
- `volta install node@10.16.0`用volta重新安装原来的版本
