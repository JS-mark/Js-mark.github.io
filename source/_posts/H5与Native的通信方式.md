---
title: H5与Native的通信方式 "JSBridge"
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2021-12-04 11:23:33
categories: 前端开发 #分类
top: 12
tags:
  - hybrid 混合式开发
  - JSBridge
  - Javascript
---

> 导读

写这篇的博客起因是由于公司app的H5 Hybrid项目引发的本文的撰写！由于公司内部JSBridge方案有些庞大，并且端上开发人力紧张等这些客观因素，简单的基于js2native的原理实现了一版简陋版JSBridge SDK，不妥之处还请大家批评指正！！！
<!-- more -->

### JSBridge起源

首先我们讲下Bridge 的起源。JSBridge 是一种 JS 实现的 Bridge，连接着桥两端的 Native 和 H5。它在 APP 内方便地让 Native 调用 JS，JS 调用 Native ，是双向通信的通道。JSBridge 主要提供了 JS 调用 Native 代码的能力，实现原生功能如查看本地相册、打开摄像头、指纹支付等。

### JSBridge 的双向通信原理

#### JS调用Native

> JS 调用 Native 的实现方式较多，目前主流采用是拦截URL Scheme 、重写prompt 、注入API方法。

基于我们的业务需求，我们仅需在鸿蒙平台下使用JSBridge，所以我们采用了拦截URL Scheme来实现，今天我们的分享也主要以这个为主！

> URL Scheme

web端采用创建隐藏的iframe进行scheme请求，端上采用拦截协议上的参数实现调用对应的native方法

```java
// 判断协议
String urlScheme = "xxx";
String callBackID = "xxx";// 从url中解析的callBackID
String data = "xxx"; // 对象字符串
boolean isSuccess = true;
if(urlScheme == 'xxx') {
    webview.executeJs("javascript:window.CallJSBridge( ," + isSuccess "," + data + "," + ")", new AsyncCallback<String>() {

    @Override

    public void onReceive(String msg) {
      // 在此确认返回结果
    }
  });
}

```

- 通过创建iframe请求URL Scheme

```ts
createIframeRequest(url: string) {
  try {
    if (!iframeNode) {
      iframeNode = documentcreateElement("iframe");
      iframeNode.style.display = "none";
      document.documentElement.appendChild(iframeNode);
      // ??是否需要删除
      let timerRemoveIframe: NodeJS.Timeout | null = setTimeout(() => {
        document.documentElement.removeChild(
          iframeNode as HTMLIFrameElement
        );
        clearTimeout(timerRemoveIframe as NodeJS.Timeout);
        timerRemoveIframe = null;
      }, 0);
    }
    // 修改url
    iframeNode.src = url;
  } catch (error) {
   // 触发异常监控
  }
}
  /**
   * 调用方法
   * @param event
   * @param data
   * @returns
   */
  invoke(event: string, data?: any) {
    return new Promise((resolve, reject) => {
      if (this.isBridgeReady) {
        reject();
        throw new Error("Bridge not ready!");
      }
      // 生成随机id
      const callback: string = nanoid();
      const dataObj = {
        event,
        callback,
      };
      const url = `${this.baseSchema}?${qs.stringify(dataObj)}`;

      // 向window上注册变量方法
      window.callbackObj[callback] = {
        url,
        call_time: +new Date(),
        success(data) {
          resolve(data);
        },
        error(error) {
          reject(error);
        },
      };

      // 请求
      this.createIframeRequest(url);

      setTimeout(() => {
        // 支持使用addJs
        if (
          window.__BridgeHandler &&
          window.__BridgeHandler.call
        ) {
          let {
            callback,
            isSuccess,
            data: res,
          } = window.__BridgeHandler.call(
            JSON.stringify({
              event,
              data,
              call_time: +new Date(),
            })
          );

          if (Object.prototype.toString.call(res) === "[object Object]") {
            res = JSON.stringify(res);
          }
          // 调用
          CallJSBridge(callback, isSuccess, res);
        }
      }, 0);
    });
  }


```

- web同学在一进入页面前注入向window中注入代码

```js
function CallJSBridge(callback, isSuccess, data) {
  // 调用window上的callback对象
  var handler = window.callbackObj[callback]
  if(handler) {
    throw new Error("handler error")
    return;
  }

  try {
    isSuccess && handler.success && handler.success(data)
    !isSuccess && handler.error && handler.error(data)
  } catch(error) {
    console.error(error)
  }
}
```

#### Native调用JS

Native 调用 JS 比较简单，只要 H5 将 JS 方法暴露在 Window 上给 Native 调用即可。

Android 和 鸿蒙OS 中主要有两种方式实现。在 4.4 以前，通过 loadUrl 方法，执行一段 JS 代码来实现。在 4.4 以后，可以使用 evaluateJavascript 方法实现。loadUrl 方法使用起来方便简洁，但是效率低无法获得返回结果且调用的时候会刷新 WebView 。evaluateJavascript 方法效率高获取返回值方便，调用时候不刷新 WebView，但是只支持 Android 4.4+。相关代码如下：

```java
/*
* 4.4 之前
*/
webView.loadUrl("javascript:" + javaScriptString);
/*
* 4.4 之后
*/
webView.evaluateJavascript(javaScriptString, new ValueCallback<String>() {
  @Override
  public void onReceiveValue(String value){
    xxx
  }
});
```

### 文档参考

- [小白必看，JSBridge 初探](https://www.zoo.team/article/jsbridge) 感谢作者
