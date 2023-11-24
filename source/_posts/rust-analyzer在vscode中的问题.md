---
title: rust-analyzer在vscode中的问题
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2023-11-24 10:33:37
tags:
  - 前端基建
  - Rust
  - rust-analyzer
categories: rust
---

## rust-analyzer在vscode中的问题.md

- 无法使用`rust-analyzer`，或者`rust-analyzer`一直在加载中
<!-- more -->

- 如果确定没有多个程序占用，可以删除rm -rf ~/.cargo/.package-cache，然后再执行cargo build 或者 cargo run
- 重启 vscode, 问题即可解决
