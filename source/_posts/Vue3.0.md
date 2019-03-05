---
layout: post
title: "初探Vue3.0新特性(未完待续)"
subtitle: "修改监察者模式、重写Virtual DOM等等等。。。"
date: 2018-12-10 12:00:00
author: "Mark"
categories: Vue #分类
header-img: "img/post-bg-js-version.jpg"
top: 7
tags:
  - 前端开发
  - JavaScript
  - Vue
---

<p align="center"><a href="#" target="_blank" rel="noopener noreferrer">
<img 
  width="200"
  height="200"
  src="https://cn.vuejs.org/images/logo.png"
  alt="Vue"></a></p>

> ### 初探 Vue3.0 新特性
>
> &emsp;“ 我已经学不动了，只有神可以挽救一下我的膝盖----” 自 2016 年 10 月 1 日 Vue2.0 版本发布以来到目前为止已经将近快两年的时间了。在这两年里，前端领域风云变化，各种框架层出不穷。小程序横空出世，angular 已经迭代到 angular6,从 angular2 开始已经基本上是将 angularjs 推倒重来，蜕变升级。等等。。。在这两年里，我们看到了太多的框架出现和消失，前端框架基本上是 vue react angular 三足鼎立。感谢各位开源大大，是你们推动了整个前端领域的快速发展。
> &emsp;与此同时，面对一时间涌现的那么多种前端框架，很多小伙伴们都会感觉力不从心，甚至还出现了众多用户到某知名开源项目上留言：“求求你别写了，我们学不动了~~”
> &emsp;今天，Vue 的主要开发者尤小右在微博上透露了 Vue3.0 的开发计划，快来看看有哪些新改变吧。

![image](/assets/img/2018/12/vue3.0.png)

> ### 9月30日，尤雨溪在medium个人博客上发布了vue3.0的开发思路，国内有翻译的版本，见文章最后的参考链接。3.0带来了很大的变化，他讲了一些改进的思路以及整个开发流程的规划。
>
> 1.Virtual DOM 完全重写，mounting & patching 提速  100% ;
> 2.更多编译时（compile-time）提醒以减少 runtime 开销;
> 3.基于 Proxy 观察者机制以满足全语言覆盖及更好的性能;
> 4.放弃 Object.defineProperty ，使用更快的原生 Proxy;
> 5.组件实例初始化速度提高 100％;
> 6.提速一倍/内存使用降低一半。

> ### 对于 3.0 的 proxy 特性有必要讲一讲
> 对于这个观察者机制的变更，给我带来的好处简直不言而喻。（我们终于不再担心目前官网上提的那个检测数组/检测对象变更了）

&emsp;不久前，也就是11月14日-16日于多伦多举办的 VueConf TO 2018 大会上，尤雨溪发表了名为 Vue3.0 Updates 的主题演讲，对 Vue3.0 的更新计划、方向进行了详细阐述（感兴趣的小伙伴可以看看完整的 [PPT](https://docs.googl初探 Vue3.0 新特性e.com/presentation/d/1yhPGyhQrJcpJI2ZFvBme3pGKaGNiLi709c37svivv0o/edit?usp=sharing)），表示已经放弃使用了 Object.defineProperty，而选择了使用更快的原生 Proxy !!
&emsp;这将会消除了之前 Vue2.x 中基于 Object.defineProperty 的实现所存在的很多限制：无法监听 属性的添加和删除、数组索引和长度的变更，并可以支持 Map、Set、WeakMap 和 WeakSet！

![image](/assets/img/2018/12/1.png)
![image](/assets/img/2018/12/2.png)

> ### 
>


最后期待，2019年的VUE3.0的发布，来让前端开发更便捷，更cool！
参考文献：
- [初探 Vue3.0 中的一大亮点——Proxy !](https://juejin.im/post/5bfcbab0518825741e7bd67f)
- [重磅！尤雨溪发布Vue 3.0开发路线](https://mp.weixin.qq.com/s/k6OhMNrpagtTmbhkW-tmZg)
- [尤大大的PPT(需要翻墙下载)](https://docs.google.com/presentation/d/1yhPGyhQrJcpJI2ZFvBme3pGKaGNiLi709c37svivv0o/edit?usp=sharing)
- [Proxy MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)