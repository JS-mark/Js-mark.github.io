---
title: JavaScript模块化语法总结 #文章页面上的显示名称，一般是中文
date: 2017-12-29 00:28:16 #文章生成时间，一般不改，当然也可以任意修改
layout: post
categories: JavaScript #分类
tags: [模块化, 规范, JS] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 服务端模块化规范 #附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
top: 3
---

# CommonJS 服务端模块化规范

# AMD/CMD 浏览器（客户端）模块化规范

```javascript
var math = require("math");

math.add(2, 3);
```
<!-- more -->
第二行 math.add(2, 3)，在第一行 require('math')之后运行，因此必须等 math.js 加载完成。也就是说，如果加载时间很长，整个应用就会停在那里等。

这对服务器端不是一个问题，因为所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间。但是，对于浏览器，这却是一个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于"假死"状态。

因此，浏览器端的模块，不能采用"同步加载"（synchronous），只能采用"异步加载"（asynchronous）。这就是 AMD 规范诞生的背景。

### AMD 规范的模块化插件（require.js 和 curl.js）

使用的是 require 导入模块

```javascript
require(['jquery', 'underscore', 'backbone'], function ($, _, Backbone){

　　　　// some code here

　　});
require会先加载jquery，underscore, backbone模块，因为这个模块化都是异步加载，加载完成后，在回调函数中调用这些模块的方法；


//指定路径

require.config({
       baseUrl:'js/lib',//放置公共路径
　　　　paths: {

　　　　　　"jquery": "jquery.min",
　　　　　　"underscore": "underscore.min",
　　　　　　"backbone": "backbone.min"

　　　　}

　　});
```

### AMD 模块规范写法

- 五、AMD 模块的写法

require.js 加载的模块，采用 AMD 规范。也就是说，模块必须按照 AMD 的规定来写。

具体来说，就是模块必须采用特定的 define()函数来定义。如果一个模块不依赖其他模块，那么可以直接定义在`define()`函数之中。

假定现在有一个 math.js 文件，它定义了一个 math 模块。那么，math.js 就要这样写：

```javascript
// math.js

define(function() {
  var add = function(x, y) {
    return x + y;
  };

  return {
    add: add
  };
}); // main.js

// 加载方法如下：

require(["math"], function(math) {
  alert(math.add(1, 1));
});
```
