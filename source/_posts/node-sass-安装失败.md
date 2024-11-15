---
title: node-sass 安装失败
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2024-11-15 17:07:37
tags:
  - 前端开发
  - JavaScript
  - NPM
  - node-sass
categories:
---

- 直接npm install时遇到sass软件报错
<!-- more -->

```bash
npm ERR! gyp ERR! stack Error: Can't find Python executable "python", you can set the PYTHON env variable.
npm ERR! gyp ERR! stack     at PythonFinder.failNoPython (D:\code\cesium-demo\node_modules\node-gyp\lib\configure.js:484:19)
npm ERR! gyp ERR! stack     at PythonFinder.<anonymous> (D:\code\cesium-demo\node_modules\node-gyp\lib\configure.js:509:16)
npm ERR! gyp ERR! stack     at callback (D:\code\cesium-demo\node_modules\graceful-fs\polyfills.js:306:20)
npm ERR! gyp ERR! stack     at FSReqCallback.oncomplete (node:fs:202:21)
npm ERR! gyp ERR! System Windows_NT 10.0.19045
npm ERR! gyp ERR! command "C:\\nvm\\nodejs\\node.exe" "D:\\code\\cesium-demo\\node_modules\\node-gyp\\bin\\node-gyp.js" "rebuild" "--verbose" "--libsass_ext=" "--libsass_cflags=" "--libsass_ldflags=" "--libsass_library="
```

- 直接使用淘宝镜像源

> 设置变量 sass_binary_site，指向淘宝镜像地址。示例：

```bash
npm i node-sass --sass_binary_site=https://registry.npmmirror.com/node-sass/

## 2024.11.5 更新
## 使用上述地址也有问题

## 可以使用以下方式
sass_binary_site=https://registry.npmmirror.com/binary.html?path=node-sass

# 也可以设置系统环境变量的方式。示例
# linux、mac 下
SASS_BINARY_SITE=https://registry.npmmirror.com/node-sass/
npm install node-sass

# window 下
set SASS_BINARY_SITE=https://registry.npmmirror.com/node-sass/ && npm install node-sass

# 或者直接全局设置
npm config set sass_binary_site npm i node-sass --sass_binary_site=https://registry.npmmirror.com/node-sass/
npm install node-sass
```

- 同时需要注意node-sass 版本和 node 版本对应关系

- 可以在此处查看

<https://www.npmjs.com/package/node-sass>
