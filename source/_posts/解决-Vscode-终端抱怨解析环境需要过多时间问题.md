---
title: 解决 Vscode 终端抱怨解析环境需要过多时间问题
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2022-07-31 12:25:36
tags:
  - 前端开发
  - Vscode
categories: 工具使用 #分类
---

当我从 Dock 启动 VSCode 时，它​​总是抱怨

> 解析您的 shell 环境需要很长时间。请
> 检查您的外壳配置。
<!-- more -->

然后稍后

> 无法在合理的时间内解析您的 shell 环境。
> 请检查您的外壳配置。

根据这个页面，[Resolving Shell Environment is Slow](https://code.visualstudio.com/docs/supporting/faq#_resolving-shell-environment-is-slow-error-warning)，如果 .bashrc 需要三秒以上，则显示第一条消息，如果需要十秒以上，则显示第二条消息。

我在 VSCode 中打开了一个终端并获取了我的 .bashrc 文件

```bash
Mark$ time source ~/.bashrc
real    0m1.448s
user    0m0.524s
sys     0m0.671s
```

如您所见，只需不到 1.5 秒。

**环境：**

- MacOS Monterey 12.5
- VSCode 1.69.2

希望有人知道是什么导致了这种情况。
除此之外，也许有人可以将我指向实际生成这些错误的代码。

遇到同样情况，发现问题：[https://github.com/microsoft/vscode/issues/113869#issuecomment-780072904](https://github.com/microsoft/vscode/issues/113869#issuecomment-780072904)

我提取`nvm load code`到问题中的`condition function`参考，解决了这个问题：

```null
function load-nvm {
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
  [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
  [[ -s `brew --prefix`/etc/autojump.sh ]] && . `brew --prefix`/etc/autojump.sh
}

# nvm
if [[ "x${TERM_PROGRAM}" = "xvscode" ]]; then
  echo 'in vscode, nvm not work; use `load-nvm`';
else
  load-nvm
fi

```
