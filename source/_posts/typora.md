---
layout: post
title: 'Typora For Markdown 语法'
date: 2018-03-12
author: 'Mark'
categories: 软件工具 #分类
top: 6
tags:
  - 前端开发
  - Markdown
---

#Typora For Markdown 语法

[Learning-Markdown (Markdown 入门参考)](http://xianbai.me/learn-md/index.html)
[TOC]

###数学表达式

要启用这个功能，首先到`Preference`->`Editor`中启用。然后使用`$`符号包裹 Tex 命令，例如：`$lim_{x \to \infty} \ exp(-x)=0$`将产生如下的数学表达式：

$\lim\_{x \to \infty} \exp(-x)=0$
<!-- more -->
###下标

下标使用`~`包裹，例如：`H~2~O`将产生 H~2~O, 即水的分子式。

###上标

上标使用`^`包裹，例如：`y^2^=4`将产生表达式 y^2^ = 4

###插入表情:happy:

使用`:happy:`输入表情:happy:,使用`:sad:`输入表情:sad:,使用`:cry:`输入表情:cry:等。以此类推！

### 下划线

用 HTML 的语法`<u>Underline</u>`将产生下划线<u>Underline</u>.

### 删除线

GFM 添加了删除文本的语法，这是标准的 Markdown 语法木有的。使用`~~`包裹的文本将会具有删除的样式，例如`~删除文本~`将产生~~删除文本~~的样式。

### 代码

- 使用`包裹的内容将会以代码样式显示，例如

```
使用`printf()`
```

则会产生`printf()`样式。

- 输入`~~~`或者```然后回车，可以输入代码块，并且可以选择代码的语言。例如：

- ````
  ​```java
  public Class HelloWorld{
    System.out.println("Hello World!");
  }
  ​```
  ````

  将会产生

  ```java
  public Class HelloWorld{
    System.out.println("Hello World!");
  }
  ```

  ### 强调

  使用两个\*号或者两个\_包裹的内容将会被强调。例如

  ```
  **使用两个*号强调内容**
  __使用两个下划线强调内容__
  ```

  将会输出

  **使用两个\*号强调内容**
  **使用两个下划线强调内容**
  Typroa 推荐使用两个\*号。

  ### 斜体

  在标准的 Markdown 语法中，\*和\_包裹的内容会是斜体显示，但是 GFM 下划线一般用来分隔人名和代码变量名，因此我们推荐是用星号来包裹斜体内容。如果要显示星号，则使用转义：

  ```
  \*
  ```

  ### 插入图片

  我们可以通过拖拉的方式，将本地文件夹中的图片或者网络上的图片插入。

  ![drag and drop image](http://typora.io/img/drag-img.gif)

  ​

  ​

### 插入 URL 连接

使用尖括号包裹的 url 将产生一个连接，例如：`<www.baidu.com>`将产生连接:<www.baidu.com>.

如果是标准的 url，则会自动产生连接，例如:www.google.com

### 目录列表 Table of Contents（TOC）

输入[toc]然后回车，将会产生一个目录，这个目录抽取了文章的所有标题，自动更新内容。

### 水平分割线

使用`***`或者`---`，然后回车，来产生水平分割线。

---

### 标注

我们可以对某一个词语进行标注。例如

```
某些人用过了才知道[^注释]
[^注释]:Somebody that I used to know.
```

将产生：

某些人用过了才知道[^注释]
[^注释]: Somebody that I used to know.

把鼠标放在`注释`上，将会有提示内容。

### 表格

```
|姓名|性别|毕业学校|工资|
|:---|:---:|:---:|---:|
|杨洋|男|重庆交通大学|3200|
|峰哥|男|贵州大学|5000|
|坑货|女|北京大学|2000|
```

将产生:

| 姓名 | 性别 |   毕业学校   | 工资 |
| :--- | :--: | :----------: | ---: |
| 杨洋 |  男  | 重庆交通大学 | 3200 |
| 峰哥 |  男  |   贵州大学   | 5000 |
| 坑货 |  女  |   北京大学   | 2000 |

其中代码的第二行指定对齐的方式，第一个是左对齐，第二个和第三个是居中，最后一个是右对齐。

### 数学表达式块

输入两个美元符号，然后回车，就可以输入数学表达式块了。例如：

```
 $$\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\\end{vmatrix}$$
```

将会产生:

$$\mathbf{V}\_1 \times \mathbf{V}\_2 = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\\frac{\partial X}{\partial u} & \frac{\partial Y}{\partial u} & 0 \\\frac{\partial X}{\partial v} & \frac{\partial Y}{\partial v} & 0 \\\end{vmatrix}$$

### 任务列表

使用如下的代码创建任务列表，在[]中输入 x 表示完成，也可以通过点击选择完成或者没完成。

```
- [ ] 吃饭
- [ ] 逛街
- [ ] 看电影
- [ ] 约泡
```

- [x] 吃饭

      ​

- [x] 逛街

      ​

- [x] 看电影

      ​

- [x] 约泡

### 列表

输入+, -, \*,创建无序的列表，使用任意数字开头，创建有序列表，例如：

```
**无序的列表**
* tfboys
* 杨洋
* 我爱你
```

**无序的列表**

- tfboys
- 杨洋
- 我爱你

```
**有序的列表**
1. 苹果
6. 香蕉
10. 我都不喜欢
```

**有序的列表**

1. 苹果
2. 香蕉
3. 我都不喜欢

### 块引用

使用>来插入块引用。例如：

```
>这是一个块引用！
```

将产生：

> 这是一个块引用！

### 标题

使用#表示一级标题，##表示二级标题，以此类推，有 6 个标题。
