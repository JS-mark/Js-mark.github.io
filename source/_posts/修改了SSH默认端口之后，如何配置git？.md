---
layout: post
title: "修改了SSH默认端口之后，如何配置git？"
date: 2020-01-19 23:24:30
author: "Mark"
categories: git操作 #分类
top: 12
tags:
  - 前端开发
  - sourceTree
  - git
---
### 出现问题

由于安全或者其它原因，我们可能会修改默认的SSH服务端口号，默认情况下，已有的git项目在pull或者push的时候会报错！

现在假设原来的项目的remote设置为git@xxx.com:Projects/xxx.git，将服务器SSH默认端口修改为223后，导致push或 pull出错
<!-- more -->
### 有两个解决办法

#### 第一种方法

```bash
git remote set-url origin ssh://git@xxx.com:223/~/Projects/p1.git
```

#### 第二种方法

```bash
cat>~/.ssh/config
# 映射一个别名
Host xxx.com
HostName xxxx.com
Port 223
AddKeysToAgent yes
UseKeychain yes
#此处是开启git的ssh翻墙代理
#ProxyCommand /usr/bin/nc -X 5 -x 127.0.0.1:1086 %h %p
IdentityFile ~/.ssh/id_rsa
```

修改p1.git项目下的git配置文件

```bash
git remote set-url origin git@xxx:Projects/p1.git
```

### 相关链接

> [gitlab 社区解决方案](https://about.gitlab.com/2016/02/18/gitlab-dot-com-now-supports-an-alternate-git-plus-ssh-port/)
