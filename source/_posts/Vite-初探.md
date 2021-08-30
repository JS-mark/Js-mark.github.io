---
title: Vite 初探
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2021-08-30 15:20:27
categories: 前端工程化 #分类
top: 12
tags:
  - 前端工程化
  - Vite
  - Javascript
---

注：转载于[前端工程化 -- vite 初探](https://juejin.cn/post/6936800551237582884)

春节期间，尤雨溪一连串的动作宣布了vite 2.0正式发布，那么赶紧来看看这个被尤雨溪号称为下一代的前端构建工具和现在的构建工具到底有哪里不一样？

vite官方介绍地址: [vitejs.dev/guide/why.h…](https://link.juejin.cn/?target=https%3A%2F%2Fvitejs.dev%2Fguide%2Fwhy.html%23slow-server-start "https://vitejs.dev/guide/why.html#slow-server-start")
<!-- more -->
首先我们要知道，vite为何而诞生？

为什么需要打包工具？
----------

在前端工程化的今天，前端的技术栈越来越丰富，配套的工具也越来越多，为了提升前端的开发效率各种各样框架是层出不穷，

但是，随着前端项目越来越大，造成了项目依赖越来越多，而这些依赖又会有着自己的依赖，这就造成了很大的一棵依赖树，打开一个项目的node\_modules目录看看，随随便便就有上百个文件夹。加上之前JavaScript 一直没有模块（module）体系，社区为此制定了一些模块加载方法，但是不同的依赖项目使用不同的加载方法。而浏览器并不支持这些加载方法，因此我们的js代码只能打包后才能在浏览器运行，因此之前的前端开发一直需要打包工具。

打包工具帮助我们实现了前端工程化，但是随着依赖越来越多，我们打包构建的速度越来越慢，并且开发中如果改动代码，热更新时还可以丢失当前正在进行的工作，vite就是为了解决这些问题而诞生的。

毫无疑问，vite最诱人的特性有以下两点：

* 极快的冷启动速度
* 极快的热更新速度

我们知道，es6中加入了模块这个特性，为的就是能有一个统一的文件加载标准，让浏览器能够根据这个标准去加载我们的代码，这样就可以提升前端开发的效率。

为何vite在冷启动和热更新性能上面可以比现代的构建工具更优秀？

冷启动
---

比如webpack，在我们往命令行中输入npm run命令把项目启动起来的时候，webpack需要把我们的依赖读出来，打包成一个一个浏览器可以识别的js文件，打包结束后，我们的服务器才真正可用。但是依赖是树状的，我们知道树的层级越深，遍历的开销就越大，这是造成webpack在热更新和冷启动上效率不高的主要原因。

我们来看看vite官网的图，webpack需要根据我们提供的文件入口去搜集依赖(由于依赖树太深，因此webpack做了很多工作希望能实现按需加载)，然后打包输出浏览器可以识别的文件，打包结束后我们的前端服务器才真正开始工作

![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7da53ccd112e4676a2c7db89afb40211~tplv-k3u1fbpfcp-watermark.awebp)

根据vite官网的图，vite是通过接受浏览器的url，来识别所需的哪些依赖，由于vite是针对es module(下文统称esm)的，es module是可以直接被浏览器识别的，因此输入命令后，项目不需要打包就可用，而且可以根据url所需的文件去返回js代码，做到了真正的按需加载。而对于非esm标准的代码，vite则是转化成esm标准的代码。 ![](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d3bfc28338f14f2ab6a25ba0f33c856c~tplv-k3u1fbpfcp-watermark.awebp)

由于webpack是把js代码打包好，当收到请求就直接返回跟浏览器(往往一个请求就可以拿到全部需要的js代码给浏览器执行，代码太多的时候则会拆分成几个文件)，而vite则是收到请求之后才去找代码，而依赖很多，可能会需要浏览器加载很多的js文件，这就可能要发很多个请求才能拿到全部的js代码，这会不会造成网络的波动？有木有可能造成页面加载的时间过长？

vite针对这种情况,在收到请求时做了一个叫pre-bundle的优化，也就是

* 把非esm的js代码转换成esm代码
* 把一个依赖打成一个一个js文件返回给前端(而不是每个文件独立返回，比如lodash-es这个依赖有600多个内部模块，最终只会返回lodash-es这一个文件，里面包含了全部的内部模块)，避免网络波动，过度占用网络端口(这里会把打包的代码放在node\_modules/.vite/下)
* 在请求js代码时加入了缓存信息在http请求的头部，实现浏览器缓存
* 对于monorepo，vite会把依赖的项目也一起通过esm的方式引入进来

热更新
---

热更新时，webpack由于是树状的依赖，因此其中的每一个节点改变，其祖先节点都需要重新编译加载(其实在应用中感觉开销比想象中更大)。并且由于热更新导致页面重新加载，页面上原本的状态也会丢失。（具体的原理还需要深入学习一下）

而vite由于基于esm，一个节点改变，大部分情况下那就只要重新请求这个节点就好了，我们知道算法中O(1)和O(n)区别还是很大的。并且vite热更新的时候，会在请求头中加入缓存的信息，服务器对于没有修改的文件返回304，极大程度的避免了不必要的重新加载

vite的其他能力
---------

### typescript

Vite仅执行.t 文件的转译工作，并不执行任何类型检查(请确保ts错误都已经处理完)。Vite 使用 esbuild 将 TypeScript 转译到 JavaScript，约是 tsc 速度的 20~30 倍

### ssr/ssg

vite的官网很明确的说vite对ssr(服务端渲染)和ssg(静态页面生成)的特性还不稳定，而且由于笔者对这两块都不怎么了解，因此暂不介绍

### sass/less

vite支持css预处理器如less和sass，如果在css文件的后缀名中加入`.module`，vite会把css文件识别成模块，于是可以像使用对象由于使用css。

```js
./index.js

import style from './style.module.scss';
<div className={style['home__title']} />

./style.module.scss
.home__title {
    color: red;
}
```

### 预构建

这里主要是两个目的

1. 性能：很多依赖，比如lodash里面有600+个js文件，如果一个一个文件请求，会产生http的开销(比如不必要的http头)，所以预构建的时候会合成一个请求。

2.转译cjs和UMD规范的代码。

### 文件缓存

为了优化开发时的性能，vite做了两个缓存。

在预构建的时候，把预构建的产物存在本地文件系统，这样即使关掉电脑，下次重启也不需要重新预构建，节省了冷启动的时间。

在浏览器请求js文件的时候，在请求头中加上缓存信息，只要js文件没有被改动，浏览器就无须重新请求文件，节省网络请求的时间。

vite的生产构建
---------

vite目前提供的生产构建方案是通过rollup来打包发布，也就是说，现在vite的优势只能发挥在开发的时候，vite不认为基于esm的构建方式适合在生产上使用。

vite官网指出，esm应用到生产上个问题，那就是http请求开销问题。一个项目涉及的js文件很容易就几百上千个，用esm逐个请求的方式会带来不必要的开销。

那么为什么不用esbuild来打生产的包？（[esbuild](https://link.juejin.cn/?target=https%3A%2F%2Fesbuild.github.io%2F "https://esbuild.github.io/")是一个基于esm的打包根据，速度比webpack快）

esbuild主要是处理js和ts文件的，在css处理方面存在问题，而且esbuild在代码分割方面也不如rollup，所以打包工作还是让更成熟的rollup来承担。

vite对比snowpack
--------------

snowpack是早于vite的一款基于es build的构建工具，vite有有一些设计是参考了snowpack的。但是青出于蓝而胜于蓝的vite有以下的优势:

* 支持多出口输出，换言之vite可以同时打包输出多个文件，这在做多入口页面应用时非常有意义
* 可以打包成库的模式(毕竟不是所有的工程都是为了输出html，比如vue只是为了输出vuejs)
* 自动分割css代码
* 异步块加载优化
* 对旧浏览器的兼容

其中，vite的生产构建是基于rollup封装好了的，而snowpack则可以用户自己决定webpack、rollup还是其他构建工具，感觉尤玉溪是真的钟爱rollup，因为vue3也是用的这个，有机会需要学习一下。

最后说一下自己的看法：vite和snowpack在我看来，区别真不算大。但是使用vite意味着现在webpack打包的那一套配置都得改，甚至可能要修改打包的流水线，某种程度上提高了使用vite的门槛，有可能会阻塞vite的推广。毕竟一项技术能否普及，首先要看市场的需要，其次是上车的门槛和社区生态。但是vite有一个天然的优势就是尤雨溪这个名字，可以让vite获得更高的曝光度，吸引更多人来关注，但是实际应用还是需要尤雨溪团队再推动一下才行。
