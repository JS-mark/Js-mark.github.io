---
title: NPM error "npm Cannot read property 'length' of undefined"
subtitle: "NPM error"
date: 2019-05-28 14:59:35
author: "Mark"
categories: JavaScript #分类
tags:
  - 前端开发
  - JavaScript
  - NPM
  - NodeJs
---

### 问题

- 出现错误版本`npm 6.9.0`

```bash
  npm -g outdated
  # 检测所有全局依赖包更新情况
```
<!-- more -->
- 报错显示

![image](/assets/img/2019/05/1.jpg)

### 修复方法

```javascript
// 148行
var columns = [
	depname,
	has || "MISSING",
	want,
	latest,
	deppath || "global" // 此处修改为这样
]
```

### 参考资料

- ["npm-outdated-throw-an-error-cannot-read-property-length-of-undefined"](https://npm.community/t/npm-outdated-throw-an-error-cannot-read-property-length-of-undefined/5929)
- ["npm Cannot read property 'length' of undefined"](https://github.com/npm/cli/commit/d07547154eb8a88aa4fde8a37e128e1e3272adc1#diff-3d20499d58f14c6f1edfe93d8ba8a8a2)
