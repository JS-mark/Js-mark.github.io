---
title: 一文彻底弄懂 "Event Loop"
type: categories
date: 2020-06-14 14:34:38
author: "Mark"
layout: post
categories: 前端面试 #分类
top: 12
external_link:
  enable: true
tags:
  - 前端开发
  - 面试题
  - Javascript
---

## 前言

> 什么是 `Event Loop` 事件循环机制？有什么作用？为什么面试经常问到？？？我在学习浏览器和NodeJS的Event Loop时翻阅了技术类型网站上大量的文章，这些文章写的都很不错、讲解的也很到位，那为什么我还是要写这篇文章呢？其实呢是由于这些文章都是针对特定的一些案例、一些情况来解释 `Event Loop`，当很多篇文章凑在一起综合来看，才可以对这些概念有较为深入的理解。
> 于是，我在看了大量文章之后，想要写这么一篇博客，不采用官方的描述，结合自己的理解以及示例代码，用最通俗的语言表达出来。希望大家可以通过这篇文章，了解到Event Loop到底是一种什么机制，浏览器和NodeJS的Event Loop又有什么区别。如果在文中出现书写错误的地方，欢迎大家留言一起探讨。(PS: 其实是很多篇文章组合在一起后才理解了这些。。。如果对你有用，就请给个Star吧~ 如有错误，欢迎指出~)
<!-- more -->

## `Event Loop` 是什么？

> `Event Loop` 是一个执行模型，在不同的地方有不同的实现。浏览器和NodeJS基于不同的技术实现了各自的 `Event Loop`。

- 浏览器的 `Event Loop` 是在html5的规范中明确定义。
- NodeJS的 `Event Loop` 是基于libuv实现的。可以参考Node的官方文档以及libuv的官方文档。

### 浏览器线程

> 我们常说 JS 是单线程语言，但是别忘了常见的浏览器内核可都是多线程的，多个线程间会进行不断通讯，通常会有如下几个线程：

- GUI 渲染进程
- JS 引擎线程
- 定时器线程
- 事件触发线程
- 异步 HTTP 请求线程

![JS EventLoop](1621f4d1b953533d.png)

- 请认真阅读以下代码，并尝试输出？

```javascript
setTimeout(function () {
  console.log('timeout1');
}, 0);

console.log('start');

Promise.resolve().then(function () {
  console.log('promise1');
  Promise.resolve().then(function () {
      console.log('promise2');
  });
  setTimeout(function () {
      Promise.resolve().then(function () {
          console.log('promise3');
      });
      console.log('timeout2')
  }, 0);
});

console.log('done');
```

### Microtask 与 Macrotask（宏队列和微队列）

> 在大多数解释 JS Event Loop 的文章中，鲜有谈及 Miscrotask 和 Macrotask 这两个概念，但这两个概念却是非常的重要，我在翻阅 Zone.js Primer  时，里面就经常会提及这两个概念，当时也是看的云里雾里的，这也是我写这篇文章的原因之一。
> Macrotask（宏队列），也叫tasks。 一些异步任务的回调会依次进入macro task queue（宏任务队列），等待后续被调用，这些异步任务包括：

- setTimeout
- setInterval
- setImmediate (Node独有)
- requestAnimationFrame (浏览器独有)
- I/O
- UI rendering (浏览器独有)

> Microtask（微队列），也叫jobs。 另一些异步任务的回调会依次进入micro task queue（微任务队列），等待后续被调用，这些异步任务包括：

- process.nextTick (Node独有)
- Promise
- Object.observe
- MutationObserver
- （注：这里只针对浏览器和NodeJS）

## 未完待续

## 参考资料

[彻底理解 JS Event Loop（浏览器环境）](https://juejin.im/post/5aa3332b518825557c011896)
[并发模型与事件循环--MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/EventLoop)
[浏览器与Node的事件循环(Event Loop)有何区别?](https://blog.fundebug.com/2019/01/15/diffrences-of-browser-and-node-in-event-loop/)
[Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
[HTLM5 EVENT LOOP DEFINITIONS](https://html.spec.whatwg.org/multipage/webappapis.html#event-loop)
[Node.js 事件循环](https://nodejs.org/zh-cn/docs/guides/event-loop-timers-and-nexttick/#what-is-the-event-loop)
