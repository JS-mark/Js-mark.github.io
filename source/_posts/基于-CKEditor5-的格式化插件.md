---
title: 基于 CKEditor5 的格式化插件
type: categories
author: Mark
comments: true
external_link:
  enable: true
date: 2022-08-22 18:46:33
categories: JavaScript #分类
tags:
  - 前端开发
  - JavaScript
  - NPM
  - CKEditor5
---

### 前言

写该文的起因是为了向大家介绍下基于CKEditor5 开发格式刷插件的思路

### 实现功能

- 实现了文字与段落的属性Copy
![image](1.gif)

### 思路

- 获取选中元素的 attribute 将其存起来
- 再次选中时将选中元素的 attribute 通过存起来的属性值将其重置（当然重置可以采用`removeFormat`插件将元素格式移除）
  - 当然选中的元素需要判别下文字用文字的方法，段落用段落的方法
  ![image](2.png)
- 关于按钮的开启状态可以通过`isEnable`去设置，通过插件的`refresh`去刷新开启状态！（P.s 当然这是我们这边的业务工需求，大家可以按实际的业务需求来做）
- UI 层的话就直接使用默认的 `Button` 按钮就好，监听下`buttonView` 然后执行`execute`
- [文档](https://ckeditor.com/docs/ckeditor5/latest/api/module_engine_model_schema-AttributeProperties.html)

### 一些实际代码

```js
/**
 * 判断是否可以开启
 */
_checkEnabled() {
  const { model } = this.editor;
  const { schema } = model;
  const selectedElements = Array.from(
    this._getNotFormattingItems(model.document.selection, schema)
  );
  const flag = selectedElements.some((item) => item.is("textProxy") || item.is("text"));
  return flag;
}


/**
 * 获取不可以参与格式化的元素
 */
* _getNotFormattingItems(selection, schema) {
  // Check formatting on selected items that are not blocks.
  for (const curRange of selection.getRanges()) {
    for (const item of curRange.getItems()) {
      if (!schema.isBlock(item)) {
        yield item;
      }
    }
  }

  // Check formatting from selected blocks.
  for (const block of selection.getSelectedBlocks()) {
    if (block) {
      yield block;
    }
  }

  // Finally the selection might be formatted as well, so make sure to check it.
  if (selection) {
    yield selection;
  }
}

```
